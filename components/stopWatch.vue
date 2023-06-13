<template>
  <div
    class="col border border-success p-2 mb-2 border-opacity-75 rounded-4 shadow p-3 mb-5 bg-body-tertiary rounded"
  >
    <h2 class="fs-3 fw-bolder">{{ titleCronometers }}</h2>
    <h3>
      <span>{{ hour }}H</span> <span>{{ min }}min</span> <span>{{ seg }}s</span>
    </h3>
    <div class="buttons-container">
      <button
        v-show="isStartButtonActive"
        class="btn btn-success"
        @click="startCronometer"
      >
        Start
      </button>
      <button
        v-show="!isStartButtonActive"
        class="btn btn-outline-info"
        @click="stopCronometer"
      >
        Stop
      </button>
      <button class="btn btn-warning" @click="resetCronometer">Reset</button>
      <button class="btn btn-danger" @click="deleteCronometer">Delete</button>
    </div>
  </div>
</template>
<script setup>
const props = defineProps({
  titleCronometers: {
    type: String,
    required: true,
  },
});
const time = ref(0);
const seg = computed(() => {
  return time.value % 60;
});
const min = computed(() => {
  return Math.floor(time.value / 60);
});
const hour = computed(() => {
  return Math.floor(time.value / 3600);
});
let cronometer = null;
let isStartButtonActive = ref(true);
const startCronometer = () => {
  isStartButtonActive.value = false;
  cronometer = setInterval(() => {
    time.value++;
  }, 1000);
};

const resetCronometer = () => {
  isStartButtonActive.value = true;
  clearInterval(cronometer);
  time.value = 0;
};
const stopCronometer = () => {
  if (cronometer !== null) {
    clearInterval(cronometer);
    isStartButtonActive.value = true;
  }
};
const emit = defineEmits(["deleteCronometer"]);
const deleteCronometer = () => {
  emit("deleteCronometer", props.index);
};
</script>
<style scoped>
.buttons-container {
  display: flex;
  justify-content: center;
  gap: 5px;
}
</style>
