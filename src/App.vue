<template>
  <Header title="Mini game - Pokemon Memories (Vue Composition)" />
  <SelecrtMode v-if="atLocation === ''" @modeSelected="selectMode" />
  <PlayMatch
    v-if="atLocation === 'match'"
    @clickBack="back2Select"
    :data="dataArr"
  />
  <Result v-if="atLocation === 'result'" />
</template>

<script setup>
import Header from "./components/Header.vue";
import SelecrtMode from "./components/SelecrtMode.vue";
import PlayMatch from "./components/PlayMatch.vue";
import Result from "./components/Result.vue";
import { ref } from "vue";

const atLocation = ref(""); // default, match, result
const dataArr = ref([]);
const selectMode = (num) => {
  const maxNum = 61; // = total images have in [images] folder
  if (num < 1) return;
  atLocation.value = "match";
  //
  dataArr.value = [];
  var eleNum = (num * num) / 2;
  while (dataArr.value.length < eleNum) {
    let i = 0;
    do {
      i = randomIntFromInterval(1, maxNum);
    } while (dataArr.value.indexOf(i) > -1);
    // console.log(i);
    dataArr.value.push(i);
  }
  dataArr.value = [...dataArr.value, ...dataArr.value];
  dataArr.value = shuffle(dataArr.value, 3);
  console.log(dataArr.value.length);
};
const shuffle = (targetArr, frequency = 1) => {
  // way 1:
  // targetArr.sort(() => randomNum());

  // way 2:
  for (let i = targetArr.length - 1; i > 0; i--) {
    // Generate random index
    const j = Math.floor(randomNum() * (i + 1));

    // Swap elements at indices i and j
    const temp = targetArr[i];
    targetArr[i] = targetArr[j];
    targetArr[j] = temp;
  }

  return targetArr;
};
const randomIntFromInterval = (min, max) => {
  return Math.floor(randomNum() * (max - min + 1) + min);
};
const randomNum = () => {
  var milliseconds = new Date().getMilliseconds();
  var n = milliseconds / 1000;
  // console.log(Math.random());
  // console.log(n);
  return n;
};

const back2Select = () => {
  atLocation.value = "";
  // console.log(atLocation.value);
};
</script>

<style>
</style>