<template>
    <div v-if="this.question">
      <h1 v-html="this.question" />
  
      <template v-for="(answer, index) in this.answers" :key="index">
        <input 
          type="radio"
          name="options"
          :value="answer"
          v-model="this.chosenAnswer"
          :disabled="this.answerSubmitted"
        />
        <label v-html="answer"></label><br />
      </template>
  
      <button v-if="!this.answerSubmitted" @click="this.submitAnswer()" class="send" type="button">Send</button>
  
      <section class="result" v-if="this.answerSubmitted">
        <h4 v-if="this.chosenAnswer === this.correctAnswer" v-html="'&#9989; Congratulations, the answer ' + this.chosenAnswer + ' is the correct one!'" />
        <h4 v-else v-html="'&#10060; I`m sorry, you picked the wrong answer. The correct is ' + this.correctAnswer + '.'" />
  
        <button @click="this.getNewQuestion()" class="send" type="button">Next question</button>
      </section>
    </div>
  </template>
  
  <script>

  export default {
    name: 'App',
  
    data() {
      return {
        question: '',
        incorrectAnswers: [],
        correctAnswer: '',
        chosenAnswer: undefined,
        answerSubmitted: false,
        userCount: 0,
        computerCount: 0
      };
    },
    
    created() {
      this.getNewQuestion();
    },
  
    computed: {
      answers() {
        var answers = [...this.incorrectAnswers];
        answers.splice(Math.round(Math.random() * answers.length), 0, this.correctAnswer);
        return answers;
      }
    },
  
    methods: {
      submitAnswer() {
        if(!this.chosenAnswer) alert("Pick one of the options!")
        else {
          this.answerSubmitted = true;
          if(this.chosenAnswer === this.correctAnswer) this.userCount++;
          else this.computerCount++;
        }

        this.$emit('updateScores', { userCount: this.userCount, computerCount: this.computerCount });
      },
  
      resetValues() {
        this.answerSubmitted = false;
        this.chosenAnswer = undefined;
        this.question = '';
      },
  
      getNewQuestion() {
        this.resetValues();
  
        this.axios.get('https://opentdb.com/api.php?amount=1&category=18')
        .then((response) => {
          this.question = response.data.results[0].question;
          this.incorrectAnswers = response.data.results[0].incorrect_answers;
          this.correctAnswer = response.data.results[0].correct_answer;
        });
      }
    }
  }
  </script>
  
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
  