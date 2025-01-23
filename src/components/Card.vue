<template>
  <div
    class="bhd-card"
    :class="[isDisabled ? 'card-disabled' : '']"
    :card-data="item"
    @click="onClickCard"
  >
    <div class="bhd-card-inner" :class="[isFlipped ? 'is-flipped' : '']">
      <div class="bhd-card-face bhd-card-front"></div>
      <div class="bhd-card-face bhd-card-rear"></div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
const isFlipped = ref(false);
const isDisabled = ref(false);
const cardPair = ref([]);

const onClickCard = (e) => {
  //   console.log("You have been click on card.")
  if (isDisabled.value) return;
  if (cardPair.value.length === 2) return;
  isFlipped.value = true;
};
</script>

<style scoped>
.bhd-card {
  background-color: cadetblue;
  width: 80px;
  height: 120px;
}
.bhd-card:hover {
  box-shadow: #01f50ddc 0px 3px 8px;
}
.bhd-card-inner {
  display: flex;
  width: 100%;
  height: 100%;
  transition: transform 1s;
  transform-style: preserve-3d;
  cursor: pointer;
  position: relative;
}
.card-disabled {
  cursor: default;
  pointer-events: none; /*disable hover events*/
}
.bhd-card-face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  overflow: hidden;
  border-radius: 8%;
  /* padding: 1rem; */
  /* box-shadow: 0 3px 18px 3px rgba(0, 0, 0, 0.2); */
  cursor: pointer;
}
.bhd-card-front {
  background: url("../assets/images/back.png") no-repeat center center;
  background-size: 80px;
  height: 100%;
  width: 100%;
}
.is-flipped {
  transform: rotateY(-180deg);
}
.bhd-card-rear {
  background-color: #7831b6;
  transform: rotateY(-180deg);
  /* background-position: center center;
  background-repeat: no-repeat;
  background-size: contain; */
  width: 100%;
  height: 100%;
}
</style>