<script setup>
import Places from "./components/Places.vue";
import Weather from "./components/Weather.vue";
import AboutUs from "./components/AboutUs.vue";
</script>

<template>
  <div class="container">
    <div id="weather"><Weather /></div>
    <div id="places"><Places /></div>
    <div id="aboutus"><AboutUs /></div>
  </div>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  #places {
    display: grid;
  }

  #weather {
    display: grid;
  }
}
</style>
