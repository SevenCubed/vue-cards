<template>
  <transition name="pop">
    <span
      v-if="visible"
      class="hand-total"
      :class="{ 'bust': total > 21, 'twenty-one': total === 21 }">
      {{ total }}
    </span>
  </transition>
</template>

<script>
export default {
  props: {
    hand: {
      type: Object,
      required: true,
    },
    total: {
        type: Number,
        required: true
    },
    visible: {
      type: Boolean,
      required: true
    }
  },
  // computed: {
  //   totaled () {
  //     // if (this.hand.length < 2) return
  //     // if (this.hand.find(card => card.faceDown)) return
  //     // return this.hand.map((c) => c.value).reduce((x, y) => x + y);
  //     //return this.hand.map(c=>c.value).reduce((x, y) => x + y);
  //     //return blackjack.score(hand.cards)
  //   }
  //}
}
</script>

<style scoped>
.hand-total {
  display: inline-block;
  position: absolute;
  top: -1.5rem;
  right: -1.5rem;
  width: 3rem;
  height: 3rem;
  font-size: 1.5rem;
  font-weight: 600;
  line-height: 2em;
  background: lightgray;
  border-radius: 50%;
  text-align: center;
  color: black;
}
.bust {
  background: darkred;
}
.twenty-one {
  background: gold;
}
.pop-enter-active {
  transition: all .3s ease;
}
.pop-leave-active {
  transition: all .1s ease;
}
.pop-enter-from, .pop-leave-to {
  transform: scale(0) rotate(360deg);
}
</style>
