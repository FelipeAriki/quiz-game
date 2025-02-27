<script setup>
import { computed, onMounted, ref, defineEmits } from 'vue';
import axios from 'axios';

const question =  ref('');
const incorrectAnswers =  ref([]);
const correctAnswer = ref('');
const chosenAnswer =  ref(undefined);
const answerSubmitted = ref(false);
const userCount = ref(0);
const computerCount = ref(0);

const emit = defineEmits(['updateScores']);

const answers = computed(() => {
    let shuffledAnswers = [...incorrectAnswers.value];
    shuffledAnswers.splice(Math.round(Math.random() * shuffledAnswers.length), 0, correctAnswer.value);
    return shuffledAnswers;
});

const submitAnswer = () => {
    if(!chosenAnswer.value) alert("Pick one of the options!")
    else {
        answerSubmitted.value = true;
        if(chosenAnswer.value === correctAnswer.value) userCount.value++;
        else computerCount.value++;
    }

    emit('updateScores', { userCount: userCount.value, computerCount: computerCount.value });
};

const resetValues = () => {
    answerSubmitted.value = false;
    chosenAnswer.value = undefined;
    question.value = '';
}

const getNewQuestion = () => {
    resetValues();

    axios.get('https://opentdb.com/api.php?amount=1&category=18')
    .then((response) => {
        question.value = response.data.results[0].question;
        incorrectAnswers.value = response.data.results[0].incorrect_answers;
        correctAnswer.value = response.data.results[0].correct_answer;
    });
}

onMounted( () => {
    getNewQuestion();
});
</script>

<template>
    <div v-if="question">
      <h1 v-html="question" />
  
      <template v-for="(answer, index) in answers" :key="index">
        <input 
          type="radio"
          name="options"
          :value="answer"
          v-model="chosenAnswer"
          :disabled="answerSubmitted"
        />
        <label v-html="answer"></label><br />
      </template>
  
      <button v-if="!answerSubmitted" @click="submitAnswer" class="send" type="button">Send</button>
  
      <section class="result" v-if="answerSubmitted">
        <h4 v-if="chosenAnswer === correctAnswer" v-html="'&#9989; Congratulations, the answer ' + chosenAnswer + ' is the correct one!'" />
        <h4 v-else v-html="'&#10060; I`m sorry, you picked the wrong answer. The correct is ' + correctAnswer + '.'" />
  
        <button @click="getNewQuestion" class="send" type="button">Next question</button>
      </section>
    </div>
  </template>
  
  <style lang="scss">
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin: 60px auto;
    max-width: 960px;
  
    input[type=radio] {
      margin: 12px 4px;
    }
  
    button.send {
      margin-top: 12px;
      height: 40px;
      min-width: 120px;
      padding: 0 16px;
      color: #fff;
      background-color: #1867c0;
      border: 1px solid #1867c0;
      cursor: pointer;
    }
  }
  </style>
  