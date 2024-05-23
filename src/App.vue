<script setup>
</script>

<template>
  <router-view :key="$route.fullPath"></router-view>
</template>

<style scoped>
* {
  margin: 0;
  padding: 0;
  list-style: none;
  text-decoration: none;
  box-sizing: border-box;
}

a {
  text-decoration: none;
}
</style>
