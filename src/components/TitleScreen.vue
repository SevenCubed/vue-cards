<template>
  <transition name="fade">
    <div class="title-screen">
        
    </div>
  </transition>
</template>
<script>

</script>

<style scoped>
.title-screen {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  width: 100%;
  display: flex;
  flex-flow: column;
  justify-content: center;
  align-items: center;
  font-size: 5rem;
  font-weight: 100;
  letter-spacing: 0.5rem;
  transition: all 0.2s;
  z-index: 200;
  background: gray;
  color: gold;
}
</style>