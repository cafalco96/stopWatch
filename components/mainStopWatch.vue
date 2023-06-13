<template>
  <h1 class="text-center">StopWatch App</h1>
  <p class="text-center">
    Control your time, create several stopwatches and be the owner of your times
    !!
  </p>
  <InputStopWatch @createdCronometer="createdCronometer"></InputStopWatch>
  <p class="text-danger text-center fs-5" v-show="errorMessage">
    {{ errorMessage }}
  </p>
  <div class="container text-center">
    <div
      v-for="(cronometer, index) in cronometers"
      :key="cronometer"
      :title="cronometer"
      :id="index"
    >
      <StopWatch
        :titleCronometers="cronometer"
        @deleteCronometer="deleteCronometer(index)"
      ></StopWatch>
    </div>
  </div>
</template>
<script setup>
let cronometers = ref([]);
const errorMessage = ref(null);
const createdCronometer = (titleCronometer, e) => {
  if (titleCronometer.value == "") {
    errorMessage.value = "Please, add a title";
  } else {
    errorMessage.value = null;
    cronometers.value.push(titleCronometer.value);
    titleCronometer.value = "";
  }
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
