<template>
  <div id="app">
    <CalculationCarLeasing />
  </div>
</template>

<script>
import CalculationCarLeasing from "@/components/CalculationCarLeasing";
export default {
  name: "App",

  components: {
    CalculationCarLeasing,
  },
};
</script>

<style lang="scss">
@import "./assets/scss/app.scss";
</style>
