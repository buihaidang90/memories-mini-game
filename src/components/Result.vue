<template>
  <div class="bhd-result-screen">
    <h1>✨ Congratulations ✨</h1>
    <h1 class="bhd-time-cost">Your time is {{ props.counter }} second</h1>
    <button @click="onStartAgain">Start Again</button>
  </div>
</template>

<script setup>
const props = defineProps({
  counter: { type: Number, require: true },
});
const emit = defineEmits(["clickBack"]);
const onStartAgain = () => {
  emit("clickBack");
};
</script>

<style scoped>
.bhd-result-screen {
  display: inline-block;
  width: 100%;
  height: 100%;
  padding-top: 40%;
  text-align: center;
}
h1{
  color: hotpink;
}
.bhd-time-cost{
  color: blue;
}
</style>