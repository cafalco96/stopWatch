<template>
  <div :class="classesStopsContainer">
    <h2 class="fs-3 fw-bolder">{{ titleCronometers.title }}</h2>
    <h3>
      <span>{{ hour }}H</span> <span>{{ min }}min</span> <span>{{ seg }}s</span>
    </h3>
    <div class="buttons-container">
      <button class="btn bg-danger-subtle" @click="resetCronometer">
        Reset
      </button>
      <button
        v-show="isStartButtonActive"
        class="btn btn-success"
        @click="startCronometer"
        :cronometerTotalTime="cronometerTotalTime"
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
      <button class="btn btn-danger" @click="deleteCronometer">Delete</button>
    </div>
  </div>
</template>
<script setup>
const props = defineProps({
  titleCronometers: {
    required: true,
  },
});
const classesStopsContainer = computed(() => {
  if (isStartButtonActive.value) {
    return [
      "border border-secondary-subtle p-2 mb-2 border-opacity-75 rounded-4 shadow p-3 mb-5 bg-body-tertiary rounded",
    ];
  } else {
    return [
      "border border-success-subtle border border-3 p-2 mb-2 border-opacity-75 rounded-4 shadow p-3 mb-5 bg-body-tertiary rounded",
    ];
  }
});

const time = ref(0);
const isStartButtonActive = ref(true);
const cronometerTotalTime = ref(0);
const seg = computed(() => {
  return time.value % 60;
});
const min = computed(() => {
  return Math.floor(time.value / 60) % 60;
});
const hour = computed(() => {
  return Math.floor(time.value / 3600);
});
let cronometer = null;
const startCronometer = () => {
  isStartButtonActive.value = false;
  cronometer = setInterval(() => {
    time.value++;
    cronometerTotalTime.value = time.value;
    saveTime();
  }, 1000);
};

const resetCronometer = () => {
  isStartButtonActive.value = true;
  clearInterval(cronometer);
  time.value = 0;
  cronometerTotalTime.value = 0;
  saveTime();
};
const stopCronometer = () => {
  if (cronometer !== null) {
    clearInterval(cronometer);
    isStartButtonActive.value = true;
    saveTime();
  }
};
const emit = defineEmits(["deleteCronometer"]);
const deleteCronometer = () => {
  emit("deleteCronometer", props);
};
onMounted(() => {
  const savedTime = localStorage.getItem(
    `cronometerTime-${props.titleCronometers.id}`
  );
  if (savedTime) {
    time.value = parseInt(savedTime, 10);
  }
});

onBeforeUnmount(() => {
  saveTime();
});

const saveTime = () => {
  localStorage.setItem(
    `cronometerTime-${props.titleCronometers.id}`,
    cronometerTotalTime.value.toString()
  );
};
</script>
<style scoped>
.buttons-container {
  display: flex;
  justify-content: space-between;
  gap: 5px;
}
</style>
