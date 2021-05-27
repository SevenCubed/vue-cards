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
display:inline-block;
padding:0.3em 1.2em;
margin:0 0.3em 0.3em 0;
border-radius:2em;
height: 4rem;
width: 8rem;
box-sizing: border-box;
text-decoration:none;
font-family:'Roboto',sans-serif;
font-weight:300;
color:#333333;
background-color:lightgray;
text-align:center;
transition: all 0.2s;
font-weight: bolder;
text-align: center;
padding: 1.5rem;
  box-shadow: 0 0.25rem 0 0 rgba(0, 0, 0, 0.25);
}

.game-button.is-disabled {
  opacity: 0.5;
}

.game-button:not(.is-disabled):not(:active):hover{
background-color:#eeeeee;
  transform: translateY(-0.25rem);
  box-shadow: 0 0.375rem 0 0 rgba(0, 0, 0, 0.25);
}
.game-button:not(.is-disabled):active{
    box-shadow: 0 0 0;
  transform: translateY(0);
}
</style>
