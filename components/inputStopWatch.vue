<template>
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
      ref="createButton"
      type="button"
      class="btn btn-outline-success"
      @click="createdCronometer"
    >
      Create
    </button>
  </div>
  <p class="text-danger text-center fs-5" v-show="errorMessage">
    {{ errorMessage }}
  </p>
</template>
<script setup>
const titleCronometer = ref("");
const errorMessage = ref(null);
const emit = defineEmits(["createdCronometer"]);
const createdCronometer = (e) => {
  if (titleCronometer.value == "") {
    errorMessage.value = "Please, add a title";
  } else {
    errorMessage.value = null;
    emit("createdCronometer", titleCronometer);
  }
};
</script>
