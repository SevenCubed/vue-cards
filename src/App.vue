<template>
  <transition-group name="shuffleMedium" tag="div">
    <section class="dealerHand hand">
      <Hand
        :hand="hand[0]"
        :result="dealer.result"
        v-on:log="log"
        v-on:lowAce="lowAce"
        v-on:total="total($event, dealer)"
      />
    </section>
    <section class="playerHand hand">
      <Hand
        :hand="hand[1]"
        :result="player.result"
        v-on:log="log"
        v-on:lowAce="lowAce"
        v-on:total="total($event, player)"
      />
    </section>
    <section class="controls">
      <Controls
        :enabledButtons="enabledButtons"
        v-on:shuffle="dealRound"
        v-on:hit="deal(false, 1)"
        v-on:stand="stand"
        v-on:reveal="revealDealer"
        v-on:wipe="wipe"
        v-on:log="log"
        v-on:options="sidebarToggle"
      />
    </section>
  </transition-group>
</template>

<script>
import Controls from "./components/Controls";
import Hand from "./components/Hand";
export default {
  name: "App",
  data() {
    return {
      ranks: "A 2 3 4 5 6 7 8 9 10 J Q K".split(" "),
      suits: "♠︎ ♥︎ ♣︎ ♦︎".split(" "),
      deck: [],
      hand: [[], []],
      enabledButtons: [true, true, true, true, true],
      draws: 0,
      player: {
        name: "Human",
        blackjack: false,
        total: 0,
        bust: false,
        handIndex: 1,
        wins: 0,
        result: null,
      },
      dealer: {
        name: "Dealer",
        blackjack: false,
        total: 0,
        bust: false,
        handIndex: 0,
        wins: 0,
        result: null,
      },
      delay: 500,
    };
  },
  computed: {
    dealerHand() {
      return this.hand[0];
    },
    playerHand() {
      return this.hand[1];
    },
  },
  created() {
    this.createDeck();
    // this.deck.reverse(); //This was for debugging low aces, a reversed deck starts with the Ace of Spades
    this.shuffleDeck();
    console.log(this.deck);
    this.dealRound();
  },
  methods: {
    createDeck() {
      this.deck = [];
      let s, r, v, i; //suit, rank, value, color, name, bitvalue, index
      for (i = 0; i < 52; i++) {
        r = i % 13; //rank
        s = this.suits[Math.floor(i / 13)]; //suit
        v = r + 1 < 10 ? r + 1 : 10; //value, capping at 10 for the face cards
        v = v == 1 ? 11 : v; //setting the Ace at a value of 11, as it is only necessary for it to be a 1 should the totals be over 21
        const suitColor = {
          "♠︎": "black",
          "♥︎": "red",
          "♣︎": "black",
          "♦︎": "red",
        };
        let card = {
          id: i,
          rank: this.ranks[r],
          suit: s,
          value: v,
          faceDown: false,
          color: suitColor[s],
        };
        this.deck.push(card);
      }
    },
    shuffleDeck() {
      const deck = this.deck;
      let m = deck.length,
        i;
      //While there remain elements to shuffle…
      while (m) {
        //Pick a remaining element…
        i = Math.floor(Math.random() * m--);
        //And swap it with the current element.
        [deck[m], deck[i]] = [deck[i], deck[m]];
      }
      return this;
    },
    deal(faceDown, index) {
      //console.log(this.hand[index]);
      let newCard = this.deck.pop();
      newCard["faceDown"] = faceDown;
      this.hand[index].push(newCard);
    },
    dealRound() {
      const q = [1, 0, 1, 0]; //player, dealer, player, dealer
      this.enabledButtons = [false, false, false, false, false];
      setTimeout(() => {
        this.enabledButtons = [true, true, true, true, true];
      }, this.delay * 4.5);
      let faceDown = false;
      q.forEach((handIndex, i) => {
        setTimeout(() => {
          i == 1 ? (faceDown = true) : (faceDown = false);
          this.deal(faceDown, handIndex);
        }, this.delay * (i + 1));
      });
    },
    revealDealer() {
      this.hand[0].forEach((card) => {
        card.faceDown = false;
      });
    },
    lowAce(card) {
      card.value = 1;
    },
    total(event, user) {
      user.total = event;
      console.log(user.total);
      //Blackjack
      if (this.hand[user.handIndex].length == 2 && user.total == 21) {
        user.blackjack = true;
        if (user == this.player) {
          setTimeout(() => {
            this.revealDealer();
          }, this.delay / 2);
          setTimeout(() => {
            this.eval();
          }, this.delay);
        }
      }
      //Bust
      if (user.total > 21) {
        user.bust = true;
        console.log("BUST");
        if (user == this.player) {
          this.enabledButtons[0] = false;
          this.enabledButtons[1] = false;
          setTimeout(() => {
            this.revealDealer();
          }, this.delay / 2);
          //go to eval, player busted
          setTimeout(() => {
            this.eval();
          }, this.delay);
        }
      }
    },
    stand() {
      this.enabledButtons = [false, false, false, false, false];
      this.revealDealer();
      console.log(this.dealer.total);
      this.dealerDraw();
    },
    dealerDraw() {
      if (
        (this.dealer.total < 17 ||
          (this.dealer.total < 16 && this.player.total == 16)) &&
        !this.player.bust &&
        !this.player.blackjack
      ) {
        setTimeout(() => {
          this.deal(false, 0);
        }, this.delay);
        setTimeout(() => {
          this.dealerDraw();
        }, this.delay + 50);
        //I'm doing it like this because the While function from my old script basically bugged out.
        //My guess it that it's to do with the fact that I emit the totals from the components, which introduces a bit of lag and allows the while function to basically loop a ton of times
        //before it actually updates and processes that the total is above 17
        //In JS I can't actually pause or wait execution (I think), and settimeout just delays whatever it's meant to execute, but keeps executing the code right after it.
        //On my first attempt the browser froze.
        //On my second attempt I was allowed to draw a total of 332, which is probably the whole deck.
        //This is also the reason that the repeated dealerDraw call is 50 ms later, to allow the total to actually update.
      } else {
        console.log("done");
        setTimeout(() => {
          this.eval();
        }, this.delay);
      }
    },
    eval() {
      // eslint-disable-next-line no-unused-vars
      let result, details;
      //Why doesn't it see these?
      switch (true) {
        case this.player.blackjack && this.dealer.blackjack:
          console.log(
            "Draw: Both the player as the dealer have a blackjack! What are the odds!?"
          );
          this.player.result = "Blackjack";
          this.dealer.result = "Blackjack";
          details = `Both ${this.player.name} as the dealer have a blackjack! What are the odds!?`;
          this.draws++;
          break;
        case this.player.blackjack:
          console.log("Victory: Player won through blackjack.");
          this.player.result = "Blackjack";
          details = `${this.player.name} won through blackjack!`;
          this.player.wins++;
          break;
        case this.dealer.blackjack:
          console.log("Defeat: dealer won through blackjack");
          this.dealer.result = "Blackjack";
          this.player.result = "Lose";
          details = "The this.dealer won through blackjack?";
          this.dealer.wins++;
          break;
        case this.player.bust:
          console.log("Defeat: Player busted.");
          this.player.result = "Bust";
          this.dealer.result = "Win";
          details = `${this.player.name} busted.`;
          this.dealer.wins++;
          break;
        case this.dealer.bust:
          console.log("Victory: dealer busted.");
          this.player.result = "Win";
          this.dealer.result = "Bust";
          details = "The this.dealer busted.";
          this.player.wins++;
          break;
        /*case (this.player.hand.length >=5 && !this.playerbust):
            console.log('Victory: Five card trick! ... But this is a casino game, so we don't use that rule.)
            this.player.wins++
            break;      */
        case this.dealer.total == this.player.total:
          console.log("Draw: dealer scored equal to the player.");
          this.player.result = "Push";
          this.dealer.result = "Push";
          details = `${this.player.name} and the dealer have the same score!`;
          this.draws++;
          break;
        case this.dealer.total > this.player.total:
          console.log("Defeat: dealer scored higher than the player.");
          this.player.result = "Lose";
          this.dealer.result = "Win";
          details = `The this.dealer scored higher than ${this.player.name}!`;
          this.dealer.wins++;
          break;
        case this.player.total > this.dealer.total:
          console.log("Victory: Player scored higher than the dealer.");
          this.player.result = "Win";
          this.dealer.result = "Lose";
          details = `${this.player.name} scored higher than the this.dealer!`;
          this.player.wins++;
          break;
        default:
          result = "Something broke.";
          details = "I dunno how you got here, but please leave.";
          break;
      }
      setTimeout(() => {
        this.wipe();
      }, this.delay * 2);
    },
    wipe() {
      this.hand = [[], []];
      this.player.blackjack = false;
      this.player.bust = false;
      this.dealer.blackjack = false;
      this.dealer.bust = false;
      this.dealer.result = null;
      this.player.result = null;
      if (this.deck.length < 34) {
        this.createDeck();
        this.shuffleDeck();
      }
      setTimeout(() => {
        this.dealRound();
      }, this.delay);
    },
    log() {
      console.log(this.dealer.total);
      this.player.result = "Blackjack";
    },
  },
  components: {
    Hand,
    Controls,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.hand {
  height: 12em;
  padding: 2em;
  display: flex;
  align-items: center;
}
.playerHand {
  flex: 1 0;
  display: flex;
  flex-flow: row nowrap;
  justify-content: space-around;
}
.dealerHand {
  margin-top: 1rem;
  display: flex;
  flex-flow: row nowrap;
  justify-content: center;
  min-height: 12.4rem;
}
.options {
  display: absolute;
  left: 10px;
  top: 5px;
}
.controls {
  padding: 2em;
  display: flex;
  flex-flow: row nowrap;
  justify-content: center;
  align-items: center;
}
.game-board {
  display: grid;
  grid-template-columns: 100px 100px 100px 100px 100px 100px 100px 100px;
  grid-column-gap: 30px;
  grid-row-gap: 30px;
  justify-content: center;
}
.shuffleMedium-move {
  transition: transform 1s;
}
</style>
