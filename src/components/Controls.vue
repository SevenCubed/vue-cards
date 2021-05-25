<template>
      <section class="game-controls">
    <GameButton
      action="deal"
      label="HIT"
      :isEnabled="true"
      v-on:deal="$emit('deal-up')"
    />
    <GameButton
      action="deal"
      label="DEALER"
      :isEnabled="true"
      v-on:deal="$emit('deal-down')"
    />
    <GameButton
      action="shuffle"
      label="NEW"
      :isEnabled="true"
      v-on:shuffle="$emit('shuffle')"
    />    
    <GameButton
      action="reveal"
      label="REVEAL"
      :isEnabled="true"
      v-on:reveal="$emit('reveal')"
    />
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
