<template>
<div>

  <ScoreBoard :winCount="winCount" :loseCount="loseCount" />

  <template v-if="this.question">
  <h1 v-html="this.question"></h1>
  
  <div style="align-items: center;">
    <div v-for="(answer, index) in this.answers" :key="index">
      <input
      :disabled="this.answerSubmitted" 
      type="radio" 
      name="options" 
      :value="answer" 
      v-model="this.chosen_answer">
      <label v-html="answer" ></label> <br>
    </div>
 

    <button v-if="!this.answerSubmitted" class="send" @click="this.submitAnswer" type="button">Send</button>


    <section v-if="this.answerSubmitted" class="results">
       <h4 v-if="this.chosen_answer == this.correctAnswer">✅ Congratulations, the answer "{{this.correctAnswer}}" is correct.</h4>
        <h4 v-else>❌ I'm sorry, you picked the wrong answer. The correct is "{{this.correctAnswer}}"  .</h4>
       <button @click="this.getNewQuestion()" class="send" type="button">Next question</button>
    </section>

  </div>

</template>
</div> 
</template>

<script>

import ScoreBoard from './components/ScoreBoard.vue'

export default {
  name: 'App',
  components:{
    ScoreBoard
  },

  data(){
    return{
      question: undefined,
      incorretAnswers: undefined,
      correctAnswer: undefined,
      chosen_answer: undefined,
      answerSubmitted: false,
      winCount: 0,
      loseCount: 0
    }
  },
 
  computed: {
    answers() {
      var answers = JSON.parse(JSON.stringify(this.incorretAnswers)); // aula 27
      answers.splice(Math.round(Math.random() * answers.length), 0, this.correctAnswer)
      
      return answers;
    }
  },

  created(){
      this.getNewQuestion()
  },

  methods:{
    submitAnswer(){
      if(!this.chosen_answer){
        alert('Pick one of the options')
      } else {
        this.answerSubmitted = true
        if(this.chosen_answer == this.correctAnswer) {
          this.winCount++;
        } else {
          this.loseCount++;
        }
      }
    },

    getNewQuestion(){
          this.question = undefined
          this.chosen_answer = undefined
          this.answerSubmitted = false

        this.axios.get('https://opentdb.com/api.php?amount=1&category=18')
        
        .then((response) => {
          this.question = response.data.results[0].question;
          this.incorretAnswers = response.data.results[0].incorrect_answers;
          this.correctAnswer = response.data.results[0].correct_answer;
    
      })
    }
  }
}  

// https://opentdb.com/api.php?amount=1&type=boolean

</script>

<style >
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  max-width: 960px;
  align-items: center;
  margin: 60px auto; 
  
}

#app input[type=radio]{
  margin: 12px 4px;
  
}

#app button.send{
  margin-top: 12px;
  height: 40px;
  min-width: 120px;
  padding: 0 16px;
  color: #fff;
  background-color: #1867c0;
  border:1px solid #1867c0;
  cursor: pointer;
}

</style>
