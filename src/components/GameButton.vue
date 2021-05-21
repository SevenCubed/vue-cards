<template>
  <a
    class="game-button button"
    :class="{ 'is-disabled': !isEnabled }"
    @click="doAction"
  >
    {{ label }}
  </a>
</template>

<script>
export default {
  props: {
    action: {
      type: String,
      required: true,
    },
    isEnabled: {
      type: Boolean,
      required: true,
    },
    label: { type: String },
  },
  methods: {
    doAction() {
      if (this.isEnabled) this.$emit(this.action);
    },
  },
};
</script>

<style scoped>
.game-button {
  display: inline-block;
  padding: 0 0.5rem;
  max-width: 8rem;
  height: 6rem;
  border: 1px black solid;
}
</style>
