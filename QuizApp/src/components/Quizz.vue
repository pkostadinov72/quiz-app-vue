<template>
  <main>
    <h2 :class="{ hidden: didQuizEnd }">
      Question {{ currentQuizId }} out of {{ quizzes.length }}
    </h2>
    <h1 :class="{ shown: didQuizEnd }" class="correctCount">
      Correct answers {{ correctCount }} out of {{ quizzes.length }}
    </h1>
    <div>
      <Question :quiz-ended="didQuizEnd" :question="question"></Question>
      <p>
        <Answers
          :quiz-ended="didQuizEnd"
          :answers="answers"
          @submit-answer="handleAnswerSubmission($event)"
        ></Answers>
      </p>
      <button
        class="finalButtons"
        :class="{ shown: didQuizEnd }"
        @click="tryAgainSameQuiz()"
      >
        Try Again Same Quiz
      </button>
      <button
        class="finalButtons"
        :class="{ shown: didQuizEnd }"
        @click="newQuiz()"
      >
        New Quiz
      </button>
    </div>
  </main>
</template>

<script setup lang="ts">
import axios from "axios";
import { ref } from "vue";

// components
import Answers from "./Answers.vue";
import Question from "./Question.vue";

type Quiz = {
  question: string;
  correctAnswer: string;
  incorrectAnswers: string[];
};

const answers = ref<string[]>([]);
const question = ref<string>();
const currentQuizId = ref<number>(0);
const quizzes = ref<Quiz[]>([]);
const correctCount = ref<number>(0);
const didQuizEnd = ref<boolean>(false);
let correctAnswer: string;

function handleAnswerSubmission(answer: string) {
  console.log(question.value);
  console.log(`You clicked ${answer} , correct answer is ${correctAnswer}`);
  console.log(`-------------------------------------------------------------`);
  if (answer === correctAnswer) {
    correctCount.value++;
  }
  nextQuestion();
}
// function delay(time: number) {
//   return new Promise((resolve) => setTimeout(resolve, time));
// }
// delay(1000).then(() => nextQuestion());

function shuffle(array: string[]) {
  array.sort(() => Math.random() - 0.5);
}

async function getQuestionsData() {
  const response = await axios.get(
    "https://the-trivia-api.com/api/questions?categories=general_knowledge,geography,food_and_drink,society_and_culture,sport_and_leisure,arts_and_literature,film_and_tv,history,science,music&limit=10"
  );
  quizzes.value = response.data;
  nextQuestion();
}

function tryAgainSameQuiz() {
  didQuizEnd.value = false;
  currentQuizId.value = 0;
  correctCount.value = 0;
  nextQuestion();
}

function newQuiz() {
  window.location.reload();
}

function nextQuestion() {
  if (currentQuizId.value === quizzes.value.length) {
    didQuizEnd.value = true;
    console.log("Quiz is done");
    console.log(
      `Correct answers ${correctCount.value} out of ${quizzes.value.length}`
    );
  } else {
    const currentQuiz = quizzes.value[currentQuizId.value];
    question.value = currentQuiz.question;
    correctAnswer = currentQuiz.correctAnswer;
    answers.value = [...currentQuiz.incorrectAnswers, correctAnswer];
    shuffle(answers.value);
    currentQuizId.value++;
  }
}
getQuestionsData();
</script>

<style>
.correctCount {
  font-family: "Lucida Sans", "Lucida Sans Regular", "Lucida Grande",
    "Lucida Sans Unicode", Geneva, Verdana, sans-serif;
  margin: 8px;
  visibility: hidden;
}

.finalButtons {
  visibility: hidden;
  display: inline-block;
  outline: none;
  cursor: pointer;
  font-weight: 600;
  border-radius: 3px;
  padding: 12px 24px;
  margin: 8px;
  border: 0;
  color: #3a4149;
  background: #e7ebee;
  line-height: 1.15;
  font-size: 15px;
  font-family: "Lucida Sans", "Lucida Sans Regular", "Lucida Grande",
    "Lucida Sans Unicode", Geneva, Verdana, sans-serif;
}

.finalButtons:hover {
  background-color: #c7c7c7;
}

.shown {
  visibility: visible;
}

.hidden {
  visibility: hidden;
}
</style>
