<template>
  <div>
    <Question :question="question"></Question>
    <p>
      <Answers
        :is-correct="(itsCorrect as boolean)"
        :answers="answers"
        @submit-answer="handleAnswerSubmission($event)"
      ></Answers>
    </p>
  </div>
</template>

<script setup lang="ts">
import axios from "axios";
import { ref } from "vue";

// components
import Answers from "./Answers.vue";
import Question from "./Question.vue";

type Quiz = {
  question: string;
  correct_answer: string;
  incorrect_answers: string[];
};

const answers = ref<string[]>([]);
const question = ref<string>();
let currentQuizId = 0;
let quizzes: Quiz[];
const itsCorrect = ref<boolean>(false);
let correctAnswer: string;

function handleAnswerSubmission(answer: string) {
  if (answer === correctAnswer) {
    itsCorrect.value = true;
  } else {
    itsCorrect.value = false;
  }
  function delay(time: number) {
    return new Promise((resolve) => setTimeout(resolve, time));
  }
  delay(1000).then(() => nextQuestion());
}

async function getQuestionsData() {
  const response = await axios.get(
    "https://opentdb.com/api.php?amount=5&category=9&difficulty=easy&type=multiple"
  );
  quizzes = response.data.results;
  nextQuestion();
}

function nextQuestion() {
  const currentQuiz = quizzes[currentQuizId];
  question.value = currentQuiz.question;
  correctAnswer = currentQuiz.correct_answer;
  answers.value = [...currentQuiz.incorrect_answers, correctAnswer];
  itsCorrect.value = false;

  function shuffle(array: string[]) {
    array.sort(() => Math.random() - 0.5);
  }
  shuffle(answers.value); // array.value is a must, bcs it will be shown in template
  currentQuizId++; // currentQuizId += 1; currentQuizId = currentQuizId + 1;
}
getQuestionsData();
</script>

<style></style>
