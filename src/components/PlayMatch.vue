<template>
  <div class="bhd-container">
    <button @click="backToSelectMode">Back to select mode</button>
    <div class="bhd-wrapper">
      <div class="bhd-left"></div>

      <div class="bhd-board-game" :class="[layoutClass]" ref="gameRef">
        <Card
          v-for="(item, index) in cardArr"
          :key="index"
          :cardData="item"
          :cardPair="cardPair"
          @flipCard="handleFlipCard"
        />
      </div>
      <div class="right"></div>
    </div>
    <h1>Play match panel {{ cardNotOpenYet }}</h1>
    <h3>Time counter: {{ timeCounter }} second</h3>

    <button @click="onHack">Hack game</button>
  </div>
</template>

<script setup>
import Card from "./Card.vue";
import { computed, ref, onBeforeMount, watch, onMounted } from "vue";

const props = defineProps({
  data: { type: Array, required: true },
});
const emit = defineEmits(["clickBack", "finished"]);
const gameRef = ref(null);
const cardArr = ref([]);
const cardPair = ref([]);
const cardNotOpenYet = ref(-1);
const timeCounter = ref(0);
let intervalId = ref();

onBeforeMount(() => {
  clearInterval(intervalId.value);
  timeCounter.value = 0;
  cardNotOpenYet.value = props.data.length / 2;

  for (let i = 0; i < props.data.length; i++) {
    const element = {
      index: i,
      num: props.data[i],
      disabled: false,
      flipped: false,
    };
    cardArr.value.push(element);
  }
});

onMounted(() => {
  intervalId.value = setInterval(() => {
    timeCounter.value++;
  }, 1000);
});

const backToSelectMode = () => {
  emit("clickBack");
};

const handleFlipCard = (cardElement, isFlipped = false) => {
  cardArr.value.forEach((item) => {
    if (item.index === cardElement.index) item.flipped = isFlipped;
  });
  if (cardPair.value.length < 2) {
    if (!isFlipped) return;
    cardPair.value.push(cardElement);
  }
  if (cardPair.value.length === 2) {
    const item0 = cardPair.value[0];
    const item1 = cardPair.value[1];
    if (item0.num === item1.num && item0.index != item1.index) {
      console.log("Right.");
      item0.disabled = true;
      item1.disabled = true;
      cardPair.value = [];
      if (cardNotOpenYet.value > 0) cardNotOpenYet.value--;
      // console.log("All of item in CardPair are disabled.");
      if (cardNotOpenYet.value === 0) handleEdnGame();
      return;
    }
    console.log("Wrong!");
    let noClickClass = "no-click";
    gameRef.value.classList.add(noClickClass);
    setTimeout(() => {
      cardPair.value.forEach((ele) => {
        cardArr.value.forEach((item) => {
          // console.log(item.index + ";" + ele.index);
          if (item.index === ele.index) {
            // console.log(`Flipped state: ${item.flipped}`);
            item.flipped = !item.flipped;
          }
        });
      });
      cardPair.value = [];
      gameRef.value.classList.remove(noClickClass);
      // console.log("All of item in CardPair are fliped");
    }, 600);
  }
};

const onHack = () => {
  cardArr.value.forEach((card) => {
    card.flipped = true;
    card.disabled = true;
  });
  timeCounter.value = 1;
  handleEdnGame();
};

const handleEdnGame = () => {
  clearInterval(intervalId.value);
  setTimeout(() => {
    emit("finished", timeCounter.value);
  }, 1000);
};

const layoutClass = computed(() => {
  if (props.data.length === 36) return "bhd-6x6";
  else if (props.data.length === 64) return "bhd-8x8";
  else if (props.data.length === 100) return "bhd-10x10";
  else return "bhd-4x4";
});

watch(cardPair, (oldValue, newValue) => {
  // console.log(
  //   "oldCardPair:" + oldValue.length + "; newCardPair: " + newValue.length
  // );
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
  pointer-events: unset;
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
.no-click {
  pointer-events: none;
}
</style>