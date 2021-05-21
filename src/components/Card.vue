
<template>
  <div class="card" :class="{ 'face-down': card.faceDown }">
    <div class="inner">
      <div class="front" :class="card.color" @click="selectCard">
        <div class="top-corner">
        <CardRank :rank="card.rank" :color="card.color" />
        <CardSuit :suit="card.suit" :color="card.color" />
      </div>
      </div>
      <div class="back gray"></div>
    </div>
  </div>
</template>

<script>
import CardRank from "./CardRank";
import CardSuit from "./CardSuit";

export default {
  props: {
    card: {
      type: Object,
      required: true,
    },
  },
  components: {
    CardRank,
    CardSuit,
  }
};
</script>


<style  scoped>
.card {
  display: inline-block;
  width: 8rem;
  height: 12rem;
  margin: 0.4rem;
  transition: all 0.2s ease;
}
.inner {
  position: relative;
  width: 100%;
  height: 100%;
}
.black {
  color: black;
  border: 5px solid black;
}
.gray {
  color: gray;
  border: 5px solid gray;
}
.red {
  color: red;
  border: 5px solid red;
}

.card .front,
.card .back {
  border-radius: 0.75rem;
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  background-size: 100%;
  background-position: center;
  background-repeat: no-repeat;
  backface-visibility: hidden;
  transition: transform 0.5s;
  transform-style: preserve-3d;
}

.card .front {
  background-color: white;
  width: 100%;
  height: 100%;
  position: absolute;
}
.card .back {
  background-color: lightgray;
  background-repeat: no-repeat;
  transform: rotateY(-180deg);
  width: 100%;
  height: 100%;
  position: absolute;
}
.card.face-down .back {
  transform: rotateY(0deg);
}
.card.face-down .front {
  transform: rotateY(180deg);
}
.card .top-corner {
  position: absolute;
  text-align: center;
  top: 0.5rem;
  left: 0.5rem;
  width: 3rem;
  height: 6.1rem;
  display: flex;
  flex-flow: column;
  justify-content: space-between;
  align-items: stretch;
}
</style>