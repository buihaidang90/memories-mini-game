<template>
  <div class="bhd-card" :class="{ 'card-disabled': props.cardData.disabled }">
    <div
      class="bhd-card-inner"
      :class="{ 'is-flipped': props.cardData.flipped }"
      @click="onToggleFlipCard"
    >
      <div class="bhd-card-face bhd-card-front">
        <div class="bhd-front-content"></div>
      </div>
      <div class="bhd-card-face bhd-card-rear">
        <div class="bhd-rear-content" :style="backgroundImageInlineStyle"></div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, ref, onMounted } from "vue";

const props = defineProps({
  cardData: Object,
  cardPair: Array,
});
const emit = defineEmits(["flipCard"]);

onMounted(() => {
  // let _path = "@../assets/images/" + props.cardData + ".png";
  // console.log("url('" + _path + "')");
});

const onToggleFlipCard = (e) => {
  //   console.log("You have been click on card.")
  if (props.cardData.disabled) return;
  // if (props.cardPair.length >= 2) return;

  emit("flipCard", props.cardData, !props.cardData.flipped);
  // console.log(props.cardData);
};

const backgroundImageInlineStyle = computed(() => {
  let inlineStyle = `background-image: url("./src/assets/images/${props.cardData.num}.png")`;
  // console.log(inlineStyle);
  return inlineStyle;
});
</script>

<style scoped>
.bhd-card {
  width: 80px;
  height: 120px;
  /* cung cấp góc nhìn cho phần tử định vị 3D */
  perspective: 500px;
}
.bhd-card:hover {
  box-shadow: #e8f808dc 2px 2px 6px;
}
.card-disabled {
  cursor: default;
  pointer-events: none; /*disable hover events*/
}

/* This container is needed to position the front and back side */
.bhd-card-inner {
  position: relative;
  transition: transform 0.6s;
  transform-style: preserve-3d;
  cursor: pointer;
  display: flex;
  width: 100%;
  height: 100%;
}
.is-flipped {
  transform: rotateY(-180deg);
}

.bhd-card-face {
  position: absolute;
  width: 100%;
  height: 100%;
  /* pointer-events: auto; */
  /* overflow: hidden; */
  border-radius: 8%;
  -webkit-backface-visibility: hidden;
  /* Safari */
  backface-visibility: hidden;
}

.bhd-card-front {
  background-color: cadetblue;
}
.bhd-front-content {
  background: url("../assets/images/back.png") no-repeat center center;
  background-size: 80px;
  width: 100%;
  height: 100%;
}

.bhd-card-rear {
  transform: rotateY(-180deg);
  background-color: #7831b6ad;
}
.bhd-rear-content {
  background-position: center center;
  background-repeat: no-repeat;
  background-size: contain;
  width: 100%;
  height: 100%;
}
</style>