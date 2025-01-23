<template>
  <div class="bhd-container">
    <button @click="backToSelectMode">Back to select mode</button>
    <div class="bhd-wrapper">
      <div class="bhd-left"></div>

      <div class="bhd-board-game" :class="[layoutClass]">
        <Card
          v-for="(item, index) in props.data"
          :key="index"
          :cardData="item"
        />
      </div>
      <div class="right"></div>
    </div>
    <h1>Play match panel</h1>
  </div>
</template>

<script setup>
import Card from "./Card.vue";
import { computed, ref } from "vue";

const props = defineProps({
  data: { type: Array, required: true },
});
const emit = defineEmits(["clickBack"]);

const backToSelectMode = () => {
  emit("clickBack");
};
const layoutClass = computed(() => {
  if (props.data.length === 36) return "bhd-6x6";
  else if (props.data.length === 64) return "bhd-8x8";
  else if (props.data.length === 100) return "bhd-10x10";
  else return "bhd-4x4";
});
</script>

<style scoped>
.bhd-container {
  width: 100%;
  height: 100%;
  background-color: aliceblue;
  text-align: center;
  position: relative;
}
.bhd-wrapper {
  display: grid;
  grid-template-columns: 1fr auto 1fr;
  grid-column-gap: 1px;
}
.bhd-board-game {
  display: grid;
  grid-row-gap: 4px;
  grid-column-gap: 4px;
  margin: auto;
}
.bhd-4x4 {
  grid-template-rows: repeat(4, 1fr);
  grid-template-columns: repeat(4, 1fr);
}
.bhd-6x6 {
  grid-template-rows: repeat(6, 1fr);
  grid-template-columns: repeat(6, 1fr);
}
.bhd-8x8 {
  grid-template-rows: repeat(8, 1fr);
  grid-template-columns: repeat(8, 1fr);
}
.bhd-10x10 {
  grid-template-rows: repeat(10, 1fr);
  grid-template-columns: repeat(10, 1fr);
}
</style>