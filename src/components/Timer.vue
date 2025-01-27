<script setup>
import { ref, useTemplateRef } from "vue";

defineProps({
  msg: String,
});

const myAudio = useTemplateRef("my-audio");
const minute = ref(25);
const second = ref(0);
let secondInterval;

function startTimer() {
  if (!secondInterval) {
    secondInterval = setInterval(() => {
      if (second.value <= 0) {
        second.value = 60;
        minute.value--;
      }
      second.value--;
      if (minute.value === 0 && second.value === 0) {
        playAudio();
        resetTimer();
      }
    }, 1000);
  }
}

function pauseTimer() {
  clearInterval(secondInterval);
  secondInterval = "";
}

function resetTimer() {
  clearInterval(secondInterval);
  secondInterval = "";
  minute.value = 25;
  second.value = 0;
}

function playAudio() {
  myAudio.value.play();
}
</script>

<template>
  <audio ref="my-audio">
    <source src="/public/alarm.mp3" type="audio/mpeg" />
  </audio>

  <div id="timer-container">
    <span :minute="minute">{{ minute < 10 ? "0" + minute : minute }}</span>
    <span>:</span>
    <span :second="second">{{ second < 10 ? "0" + second : second }}</span>
  </div>

  <div id="control-container">
    <button @click="startTimer">start</button>
    <button @click="pauseTimer">pause</button>
    <button @click="resetTimer">reset</button>
  </div>
</template>

<style scoped>
#timer-container,
#control-container {
  /* background-color: yellow; */
  display: flex;
  width: 320px;
  justify-content: space-evenly;
}
span {
  font-size: 96px;
  font-weight: 700;
}
</style>
