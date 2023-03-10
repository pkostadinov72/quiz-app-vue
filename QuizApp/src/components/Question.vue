<template>
  <div :class="{ didQuizEnd: quizEnded }" class="questionText">
    {{ question }}
  </div>
</template>

<script setup lang="ts">
const props = defineProps<{
  question?: string;
  quizEnded: boolean;
}>();
</script>

<style>
.questionText {
  font-family: "Lucida Sans", "Lucida Sans Regular", "Lucida Grande",
    "Lucida Sans Unicode", Geneva, Verdana, sans-serif;
  font-size: 1.5rem;
  margin: 10px 5px 30px;
  text-align: left;
}

.didQuizEnd {
  visibility: hidden;
}
</style>
