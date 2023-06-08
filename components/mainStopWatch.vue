<template>
  <h1 class="text-center">StopWatch App</h1>
  <div class="container-input input-group input-group-lg">
    <span class="input-group-text" id="inputGroup-sizing-lg">Title:</span>
    <input
      v-model="titleCronometer"
      type="text"
      class="form-control"
      aria-label="Sizing example input"
      aria-describedby="inputGroup-sizing-lg"
    />
    <button
      type="button"
      class="btn btn-outline-success"
      @click="createdCronometer"
    >
      Create
    </button>
  </div>
  <div class="container text-center">
    <div
      v-for="(cronometer, index) in cronometers"
      :key="index"
      :id="index"
      :title="cronometer"
      class="col border border-success p-2 mb-2 border-opacity-75 rounded-4 shadow p-3 mb-5 bg-body-tertiary rounded"
    >
      <p class="fs-3 fw-bolder">{{ cronometer }}</p>
      <StopWatch @deleteCronometer="deleteCronometer"></StopWatch>
    </div>
  </div>
</template>
<script setup>
let cronometers = ref([]);
const titleCronometer = ref("");
const createdCronometer = () => {
  cronometers.value.push(titleCronometer.value);
  titleCronometer.value = "";
  save();
};
const deleteCronometer = (e) => {
  for (let i = 0; i < cronometers.value.length; i++) {
    if (cronometers.value[i] == e.target.parentNode.parentNode.title) {
      cronometers.value.splice(i, 1);
    }
    save();
  }
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
