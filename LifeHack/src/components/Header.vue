<template>
  <div class="logo"><img :src="'src/assets/black_logo.png'" /></div>
</template>

<script>
export default {
  name: "Header",
};
</script>

<style scoped>
.logo {
  z-index: 4;
  color: white;
  background-color: #faf9f6;
  width: 100%;
  padding-top: 2vh;
  padding-bottom: 1vh;
}

.logo img {
  object-fit: contain;
  width: 20%;
  height: 6vh;
  float: left;
}
</style>
