<template>
  <button
    class="btnAnswer"
    v-for="item in answers"
    @click="emits('submitAnswer', item)"
  >
    {{ item }}
  </button>
</template>

<script setup lang="ts">
const props = defineProps(["answers"]);
const emits = defineEmits(["submitAnswer", "changeStyle"]);
</script>

<style>
.btnAnswer {
  display: inline-block;
  outline: 0;
  cursor: pointer;
  text-align: center;
  border: 1px solid #babfc3;
  padding: 7px 16px;
  margin: 8px;
  min-height: 36px;
  min-width: 36px;
  color: #202223;
  background: #ffffff;
  border-radius: 4px;
  font-weight: 500;
  font-size: 14px;
  box-shadow: rgba(0, 0, 0, 0.05) 0px 1px 0px 0px;
  font-family: "Lucida Sans", "Lucida Sans Regular", "Lucida Grande",
    "Lucida Sans Unicode", Geneva, Verdana, sans-serif;
}

.correct {
  background: #00ff77;
}

.wrong {
  background: #ed0739;
}
</style>
