<template>
  <button v-for="item in answers">{{ item }}</button>
</template>

<script setup lang="ts">
const props = defineProps(["answers"]);
</script>

<style></style>
