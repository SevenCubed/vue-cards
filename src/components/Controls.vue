<template>
  <section class="game-controls">
    <GameButton
      action="deal"
      label="HIT"
      :isEnabled="enabledButtons[0]"
      v-on:deal="$emit('deal-up')"
    />
    <GameButton
      action="deal"
      label="DEALER"
      :isEnabled="enabledButtons[1]"
      v-on:deal="$emit('deal-down')"
    />
    <GameButton
      action="shuffle"
      label="NEW"
      :isEnabled="enabledButtons[2]"
      v-on:shuffle="$emit('shuffle')"
    />
    <GameButton
      action="reveal"
      label="REVEAL"
      :isEnabled="enabledButtons[3]"
      v-on:reveal="$emit('reveal')"
    />
    <GameButton
      action="wipe"
      label="WIPE"
      :isEnabled="enabledButtons[4]"
      v-on:wipe="$emit('wipe')"
    />
        <GameButton
      action="log"
      label="LOG"
      :isEnabled="enabledButtons[4]"
      v-on:log="$emit('log')"
    />
  </section>
</template>

<script>
import GameButton from "./GameButton";
export default {
  props: {
    enabledButtons: { type: Array, required: true },
  },
  components: {
    GameButton,
  },
};
</script>
