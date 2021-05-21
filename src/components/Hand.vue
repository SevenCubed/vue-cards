<template>
  <div
    class="game-hand"
  >
    <transition-group name="deal" tag="div" class="cards">
        <Card
          v-for="card in hand"
          :key="card.id"
          :card="card"
        />
    </transition-group>
  </div>
</template>

<script>
import Card from "./Card";

export default{
    props: {
        hand: {
            type: Array,
            required: true
        },
        faceDown: {
            type: Boolean,
            required: false,
            default: false
        }
    },
      components: {
    Card,
  },
}
</script>


<style scoped>
.game-hand {
  position: absolute;
  transition: transform 0.2s ease;
}

.game-hand.is-active, .game-hand.is-split.is-active {
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
