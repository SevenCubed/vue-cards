<template>
  <div class="suit">
    <svg style="display: none;">
      <symbol id="♣︎" viewBox="0 0 45.33 50.5">
        <circle cx="32.49" cy="29.52" r="11.69" coloropacity="1"/>
        <circle cx="22.5" cy="12.06" r="11.69" coloropacity="1"/>
        <circle cx="12.51" cy="29.52" r="11.69" coloropacity="1"/>
        <polygon points="22.5 37.09 14.73 50.36 30.26 50.36 22.5 37.09" coloropacity="1"/>
      </symbol>
      <symbol id="♦︎" viewBox="0 0 45.33 50.5" >
        <polygon points="22 50.5 38.51 22.22 5.49 22.22 22 50.5" coloropacity="1"/>
        <polygon points="22 0.5 5.49 28.78 38.51 28.78 22 0.5" coloropacity="1"/>
      </symbol>
      <symbol id="♥︎" viewBox="0 0 45.33 50.5" >
        <circle cx="11.33" cy="11.33" r="11.33" coloropacity="1"/>
        <circle cx="34" cy="11.33" r="11.33" coloropacity="1"/>
        <polygon points="22.66 50 45.17 11.46 0.16 11.46 22.66 50" coloropacity="1"/>
      </symbol>
      <symbol id="♠︎" viewBox="0 0 45.33 50.5">
        <circle cx="32.33" cy="34.2" r="10" coloropacity="1"/>
        <circle cx="12.33" cy="34.2" r="10" coloropacity="1"/>
        <polygon points="22.33 0 2.38 34.17 42.29 34.17 22.33 0" coloropacity="1"/>
        <polygon points="22.33 38.65 15.69 50 28.97 50 22.33 38.65" coloropacity="1"/>
      </symbol>
    </svg>
    <svg>
      <use :xlink:href="'#'+suit" :class="color" fill="currentColor"></use>
    </svg>
  </div>
</template>

<script>
export default {
  props: {
    suit: {
      type: String,
      required: true
    },
    color: {
        type: String,
        required: true
    }
  },
}
</script>

<style scoped>
.suit svg {
  width: 100%;
  height: 3rem;
}
</style>
