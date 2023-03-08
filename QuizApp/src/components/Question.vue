<template>
  <div class="questionText">{{ question }}</div>
</template>

<script setup lang="ts">
const props = defineProps(["question"]);
</script>

<style>
.questionText {
  font-family: "Lucida Sans", "Lucida Sans Regular", "Lucida Grande",
    "Lucida Sans Unicode", Geneva, Verdana, sans-serif;
  font-size: 1.5rem;
  margin: 10px;
}
</style>
