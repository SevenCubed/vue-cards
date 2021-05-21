<template>
  <h1>Vue cards</h1>
  <Controls
    v-on:shuffle="shuffleDeck"
    v-on:deal-up="deal(false,1)"
    v-on:deal-down="deal(true,0)"
  />
  <transition-group :name="shuffleSpeed" tag="div">
    <section class="dealer-hand game-board">
      <Hand :hand="hand[0]" />
    </section>
    <br />
    <section class="player-hand game-board">
      <Hand :hand="hand[1]" />
    </section>
    <br />
    <section class="game-board">

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
      shuffleSpeed: "shuffleMedium",
      hand: [[],[]],
    };
  },
  created() {
    this.displayInitialDeck();
    this.shuffleDeck();
    console.log(this.deck);
  },
  methods: {
    displayInitialDeck() {
      this.deck = [];
      let s, r, i; //suit, rank, value, color, name, bitvalue, index
      for (i = 0; i < 52; i++) {
        r = i % 13; //rank
        s = this.suits[Math.floor(i / 13)]; //suit
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
      console.log(this.hand[index])
      let newCard = this.deck.pop();
      newCard["faceDown"] = faceDown;
      this.hand[index].push(newCard);
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
