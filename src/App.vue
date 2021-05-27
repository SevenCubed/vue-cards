<template>
  <h1>Vue cards</h1>
  <transition-group name="shuffleMedium" tag="div">
    <section class="dealer hand">
      <Hand
        :hand="hand[0]"
        v-on:log="log"
        v-on:blackjack="blackjack(dealer)"
        v-on:bust="bust(dealer)"
        v-on:lowAce="lowAce"
      />
    </section>
    <section class="player hand">
      <Hand
        :hand="hand[1]"
        v-on:log="log"
        v-on:blackjack="blackjack(player)"
        v-on:bust="bust(player)"
        v-on:lowAce="lowAce"
      />
    </section>
    <section class="controls">
      <Controls
        :enabledButtons="enabledButtons"
        v-on:shuffle="dealRound"
        v-on:deal-up="deal(false, 1)"
        v-on:deal-down="deal(false, 0)"
        v-on:reveal="revealDealer"
        v-on:wipe="wipe"
        v-on:log="log"
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
    };
  },
  computed: {
    dealer() {
      return this.hand[0];
    },
    player() {
      return this.hand[1];
    },
  },
  created() {
    this.displayInitialDeck();
    this.deck.reverse();
    //this.shuffleDeck();
    console.log(this.deck);
    this.dealRound();
  },
  methods: {
    displayInitialDeck() {
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
      const q = [1, 0, 1, 0];
      this.enabledButtons = [false, false, false, false, false];
      setTimeout(() => {
        this.enabledButtons = [true, true, true, true, true];
      }, 2300);
      let faceDown = false;
      q.forEach((handIndex, i) => {
        setTimeout(() => {
          i == 1 ? (faceDown = true) : (faceDown = false);
          this.deal(faceDown, handIndex);
        }, 500 * (i + 1));
      });
    },
    revealDealer() {
      this.dealer.forEach((card) => {
        card.faceDown = false;
      });
      //this.hand[0][0].faceDown = false;
    },
    lowAce(card) {
      card.value = 1;
      console.log("lowered Ace?");
    },
    // eslint-disable-next-line no-unused-vars
    blackjack(user) {
      console.log(user);
      console.log("Blackjack")
    },
    // eslint-disable-next-line no-unused-vars
    bust(user) {
      console.log(user);
      console.log("Bust!")
    },
    wipe() {
      this.hand = [[], []];
    },
    log(e) {
      console.log(e);
      console.log(this.player.hand)
      console.log(this.player.name)
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
.player {
  flex: 1 0;
  display: flex;
  flex-flow: row nowrap;
  justify-content: space-around;
}
.dealer {
  margin-top: 1rem;
  display: flex;
  flex-flow: row nowrap;
  justify-content: center;
  min-height: 12.4rem;
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
