<template>
  <!-- {{ questionData.correct_answer }}
  =======================
  {{ answersArray }} -->
  <div>
    <Question :question="questionData.question"></Question>
    <p>
      <Answers :answers="answersArray"></Answers>
    </p>
  </div>
</template>

<script setup lang="ts">
import axios from "axios";
import { ref } from "vue";

// components
import Answers from "./Answers.vue";
import Question from "./Question.vue";

type questionForm = {
  question: string;
  correct_answer: string;
  incorrect_answers: string[];
};

const responseData = ref<questionForm[]>([]);
const answersArray = ref<string[]>([]);
const questionData = ref<questionForm[]>([]);

async function getQuestionsData() {
  const response = await axios.get(
    "https://opentdb.com/api.php?amount=5&category=9&difficulty=easy&type=multiple"
  );
  const resultsList = response.data.results;

  let result = 0;
  responseData.value = resultsList;
  questionData.value = resultsList[result];
  answersArray.value = resultsList[result].incorrect_answers;
  answersArray.value.push(resultsList[result].correct_answer);

  function shuffle(array: any) {
    array.value.sort(() => Math.random() - 0.5);
  }
  shuffle(answersArray);
}
getQuestionsData();
</script>

<style></style>
