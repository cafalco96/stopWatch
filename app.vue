<template>
  <div class="main">
    <MainStopWatch></MainStopWatch>
  </div>
  <FooterStopWatch></FooterStopWatch>
</template>
<script setup>
import FooterStopWatch from "./components/footerStopWatch.vue";

useHead({
  title: "StopWatch",
  link: [
    {
      rel: "icon",
      href: "public/Time-icon.svg",
    },
  ],
});
</script>
<style>
body {
  position: relative;
  padding-bottom: 7rem;
  min-height: 100vh;
}
</style>
