<template>
  <div class="game-hand">
    <transition-group name="deal" tag="div" class="cards">
      <Card v-for="card in hand" :key="card.id" :card="card" />
    </transition-group>
    <Total :total="total" :visible="visible" :hand="hand" />
    <Result :result="result"/>
  </div>
</template>

<script>
import Card from "./Card";
import Total from "./Total";
import Result from "./Result";
export default {
  props: {
    hand: {
      type: Array,
      required: true,
    },
    faceDown: {
      type: Boolean,
      required: false,
      default: false,
    },
    result: {
      type: String,
      required: false,
    }
  },
  components: {
    Card,
    Total,
    Result,
  },
  computed: {
    total() {
      if (this.hand.length < 2) return NaN;
      let valueMap = this.hand.map((card) => card.value);
      let total = valueMap.reduce((x, y) => x + y);
      let lowAce = 0;
      if (total > 21) {
        if (valueMap.indexOf(11) != -1) {
          this.$emit("lowAce", this.hand[valueMap.indexOf(11)]);
          lowAce = 10; //This needs to stay because it won't update on the first tick of the low Ace emit
          console.log("Low Ace detected, substracting 10");
        }
        // if (total - lowAce > 21) this.$emit("bust"); //Bust
      }
      this.$emit("total", total - lowAce);
      return total - lowAce;
    },
    visible() {
      if (this.hand.length < 2) return false;
      if (this.hand.find((card) => card.faceDown)) return false;
      return true;
    },
  },
};
</script>

<style scoped>
.game-hand {
  position: absolute;
  transition: transform 0.2s ease;
}

.game-hand.is-active,
.game-hand.is-split.is-active {
  position: absolute;
  max-width: 55%;
  z-index: 100;
}

.cards {
  min-height: 12.4rem;
  min-width: 8.4rem;
  display: flex;
  flex-flow: row wrap;
  justify-content: center;
  align-items: center;
}
.deal-enter-active {
  animation: deal 0.3s;
}
.deal-leave-active {
  animation: deal 0.3s reverse;
}

@keyframes deal {
  0% {
    transform: translateY(-100vh);
  }
  100% {
    transform: translateY(0);
  }
}
</style>
