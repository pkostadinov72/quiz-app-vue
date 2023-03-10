<template>
  <button
    :class="{ didQuizEnd: quizEnded }"
    class="btnAnswer"
    v-for="item in answers"
    @click="emits('submitAnswer', item)"
  >
    {{ item }}
  </button>
</template>

<script setup lang="ts">
const props = defineProps<{
  answers?: string[];
  quizEnded: boolean;
}>();
const emits = defineEmits(["submitAnswer"]);
</script>

<style>
.btnAnswer {
  display: inline-block;
  outline: none;
  cursor: pointer;
  font-weight: 600;
  border-radius: 3px;
  padding: 12px 24px;
  margin: 8px 12px;
  border: 0;
  color: #3a4149;
  background: #e7ebee;
  line-height: 1.15;
  font-size: 15px;
  font-family: "Lucida Sans", "Lucida Sans Regular", "Lucida Grande",
    "Lucida Sans Unicode", Geneva, Verdana, sans-serif;
}

.btnAnswer:hover {
  background-color: #c7c7c7;
}

.didQuizEnd {
  visibility: hidden;
}
</style>
