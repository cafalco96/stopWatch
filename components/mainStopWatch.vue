<template>
  <h1 class="text-center">StopWatch App</h1>
  <p class="text-center">
    Control your time, create several stopwatches and be the owner of your times
    !!
  </p>
  <InputStopWatch @createdCronometer="createdCronometer"></InputStopWatch>
  <div class="container text-center">
    <div
      v-for="(cronometer, index) in cronometers"
      :key="cronometer.id"
      :title="cronometer.title"
      :id="cronometer.id"
    >
      <StopWatch
        :titleCronometers="cronometer"
        @deleteCronometer="deleteCronometer(index)"
      ></StopWatch>
    </div>
  </div>
</template>
<script setup>
import { v4 as uuidv4 } from "uuid";
const cronometers = ref([]);
const createdCronometer = (titleCronometer) => {
  cronometers.value.push({ title: titleCronometer.value, id: uuidv4() });
  titleCronometer.value = "";
};
const deleteCronometer = (index) => {
  cronometers.value.splice(index, 1);
};
const save = () => {
  localStorage.setItem("cronometers", JSON.stringify(cronometers.value));
};

onMounted(() => {
  const savedCronometer = localStorage.getItem("cronometers");
  if (savedCronometer) {
    cronometers.value = JSON.parse(savedCronometer);
  }
});
watch(
  () => cronometers.value,
  () => {
    save();
  },
  { deep: true }
);
</script>
<style scoped>
.container-input {
  padding: 15px 45px;
}
.container {
  margin-top: 25px;
}
@media (min-width: 768px) {
  .container {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 25px;
  }
}
</style>
