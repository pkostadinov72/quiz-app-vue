<template>
  <div>{{ questionsArray }}</div>
</template>

<script setup lang="ts">
import axios from "axios";
import { ref } from "vue";

const responseData: any = ref();
const questionsArray: any = ref([]);
// const answersArray: any = ref([])

async function getQuestionsData() {
  const response = await axios.get(
    "https://opentdb.com/api.php?amount=5&category=9&difficulty=easy&type=multiple"
  );
  responseData.value = response.data.results;
  for (let result in response.data.results) {
    questionsArray.value.push(response.data.results[result].question);
  }
}
getQuestionsData();
</script>

<style></style>
