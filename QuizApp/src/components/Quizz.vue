<template>
  <h2>Question {{ currentQuizId }} out of {{ quizzes.length }}</h2>
  <div>
    <Question :question="question"></Question>
    <p>
      <Answers
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
const currentQuizId = ref<number>(0);
const quizzes = ref<Quiz[]>([]);
const correctCount = ref<number>(0);
let correctAnswer: string;

function handleAnswerSubmission(answer: string) {
  if (answer === correctAnswer) {
    correctCount.value++;
  } else {
    alert("wrong");
  }
  nextQuestion();
}
// function delay(time: number) {
//   return new Promise((resolve) => setTimeout(resolve, time));
// }
// delay(1000).then(() => nextQuestion());

async function getQuestionsData() {
  const response = await axios.get(
    "https://opentdb.com/api.php?amount=5&category=9&difficulty=easy&type=multiple"
  );
  quizzes.value = response.data.results;
  nextQuestion();
}

function nextQuestion() {
  if (currentQuizId.value === quizzes.value.length) {
    console.log("Quiz is done");
  } else {
    const currentQuiz = quizzes.value[currentQuizId.value];
    question.value = currentQuiz.question;
    correctAnswer = currentQuiz.correct_answer;
    answers.value = [...currentQuiz.incorrect_answers, correctAnswer];
    function shuffle(array: string[]) {
      array.sort(() => Math.random() - 0.5);
    }
    shuffle(answers.value);
    currentQuizId.value++;
  }
}
getQuestionsData();
</script>

<style></style>
