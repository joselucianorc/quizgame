<template>

  <template v-if="this.question">

    <h1 v-html="this.question"></h1>

    <template v-for="(answer, index) in this.answers" v-bind:key="index">
      <input
        :disabled="this.answerSubmitted"
        type="radio"
        name="options"
        v-bind:value="answer"
        v-model="this.chosenAnswer">
  
        <label v-html="answer"></label><br>
    </template>

  </template>

  <button @click="this.submitMethod()" class="send" type="button">Send</button>

  <section v-if="this.answerSubmitted" class="result">
    
    <template v-if="this.chosenAnswer == this.correctAnswer">
      <h4>&#9989; Congratulations, the answer ' + this.correctAnswer + ' is correct.</h4>
    </template>

    <template v-else>
      <h4>Im sorry, you picked the wrong answer. The correct answer is {{ this.correctAnswer }}</h4>
    </template>

    <button @click="this.getNewQuestion()" class="send" type="button">Next question</button>
  </section>

</template>

<script>

export default {
  name: 'App',

  components: {
  },

  computed: {
    answers() {
      //var answers = JSON.parse( JSON.stringify( this.incorrectAnswers ) );
      var answers = [...this.incorrectAnswers];
      answers.splice(Math.round(Math.random() * answers.length), 0, this.correctAnswer);
      //answers.push(this.correctAnswer);
      return answers; 
    }
  },

  data() {
    return {
      question: undefined,
      incorrectAnswers: undefined,
      correctAnswer: undefined,
      chosenAnswer: undefined,
      answerSubmitted: false
    }
  },
  created() {
    this.getQuestion();
  },
  methods: {
    submitMethod() {

      if (!this.chosenAnswer) {
        console.log('Choose one of the options');
      } else {
        this.answerSubmitted = true;
        if (this.chosenAnswer == this.correctAnswer) {
          console.log('You got it right');
        } else {
          console.log('You got it wrong');
        }
      }
    },
    getQuestion() {
      this.axios
          .get('https://opentdb.com/api.php?amount=1&category=18').then((response) => {
            
            this.question = response.data.results[0].question;
            this.incorrectAnswers = response.data.results[0].incorrect_answers;
            this.correctAnswer = response.data.results[0].correct_answer;

            console.log('question: ' + this.question);
            console.log('incorrects: ' + this.incorrectAnswers);
            console.log('correct: ' + this.correctAnswer)

            console.log(this.incorrectAnswers);
            console.log(this.answers);
          })
    },
    getNewQuestion() {
      console.log('Question sent');
      this.chosenAnswer = undefined;
      this.answerSubmitted = false;
      this.getQuestion();
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
  margin-top: 60px auto;
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
