<template>
  <h2>
    <span>{{ hour }}H</span> <span>{{ min }}min</span> <span>{{ s }}s</span>
  </h2>
  <div class="buttons-container">
    <button
      v-show="buttonStatus"
      class="btn btn-success"
      @click="startCronometer"
    >
      Start
    </button>
    <button
      v-show="!buttonStatus"
      class="btn btn-outline-info"
      @click="stopCronometer"
    >
      Stop
    </button>
    <button class="btn btn-warning" @click="resetCronometer">Reset</button>
    <button class="btn btn-danger" @click="deleteCronometer">Delete</button>
  </div>
</template>
<script setup>
let s = ref(0);
let min = ref(0);
let hour = ref(0);
let cronometer = null;
let buttonStatus = ref(true);
const startCronometer = () => {
  buttonStatus.value = false;
  cronometer = setInterval(() => {
    s.value++;
    if (s.value === 60) {
      s.value = 0;
      min.value++;
    }
    if (min.value === 60) {
      min.value = 0;
      hour.value++;
    }
  }, 1000);
};
const resetCronometer = () => {
  buttonStatus.value = true;
  clearInterval(cronometer);
  s.value = 0;
  min.value = 0;
  hour.value = 0;
};
const stopCronometer = () => {
  if (cronometer !== null) {
    clearInterval(cronometer);
    buttonStatus.value = true;
  }
};
const emit = defineEmits(["deleteCronometer"]);
const deleteCronometer = (e) => {
  emit("deleteCronometer", e);
};
</script>
<style scoped>
.buttons-container {
  display: flex;
  justify-content: center;
  gap: 5px;
}
</style>
