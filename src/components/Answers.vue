<template>
  <button v-for="item in answers" @click="emits('submitAnswer',item)">{{ item }}</button>
</template>

<script setup lang="ts">
const props = defineProps(["answers"]);
const emits = defineEmits(['submitAnswer'])
</script>

<style></style>
