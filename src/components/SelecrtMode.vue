<template>
  <div class="select-overlay">
    <div class="select-panel">
      <div class="select-btn">
        <button
          class="btn-mode"
          v-for="btn in btnArr"
          :key="btn.num"
          :data="btn.num"
          @click="selectMode"
        >
          {{ btn.num }} x {{ btn.num }}<br />{{ btn.text }}
        </button>
      </div>
      <p class="select-text">select mode to start mini game</p>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const btnArr = ref([
  { num: 4, text: "Easy" },
  { num: 6, text: "Normal" },
  { num: 8, text: "Hard" },
  { num: 10, text: "Expert" },
]);

const emit = defineEmits(["modeSelected"]);
const selectMode = (e) => {
  const num = e.currentTarget.attributes["data"].value;
//   console.log("Your selected=" + num);
  emit("modeSelected", num);
};
</script>

<style scoped>
.select-overlay {
  width: 100%;
  height: 100%;
  text-align: center;
}
.select-panel {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  left: 50%;
  transform: translateX(-50%);
}
.select-btn {
  display: inline-flex;
}
.btn-mode {
  width: 150px;
  height: 150px;
  margin: 0 1px;
  border: none;
  border-radius: 10%;
  background-color: darkblue;
  color: #fff;
  text-align: center;
  cursor: pointer;
}
.btn-mode:hover {
  background-color: #0d0dcfe0;
  color: chartreuse;
}
.btn-text {
}
.select-text {
  color: #033826;
  margin: 0;
  padding: 0;
}
</style>