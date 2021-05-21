<template>
      <section class="game-controls">
    <GameButton
      action="deal"
      label="deal face up"
      :isEnabled="true"
      v-on:deal="$emit('deal-up')"
    />
    <GameButton
      action="deal"
      label="deal face down"
      :isEnabled="true"
      v-on:deal="$emit('deal-down')"
    />
    <GameButton
      action="shuffle"
      label="shuffle"
      :isEnabled="true"
      v-on:shuffle="$emit('shuffle')"
    />
    <!--  <button @click="shuffleDeck" class="button">Shuffle</button>
    <button @click="deal" class="button">Deal</button>-->
  </section>
</template>

<script>
import GameButton from "./GameButton";
export default {
  components: {
    GameButton,
  },
};
</script>
