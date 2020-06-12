<template>
  <div id="app">
    <Header :correctAnswers="correctAnswers" :number="index + 1"/>
    
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3"> 
          <Question v-if="questions.length" :increment="increment" :answers="answers" :number="index + 1" :next="next" :question="questions[index]" />
        </b-col>
      </b-row>
    </b-container>
    

  </div>
</template>

<script>
import Header from './components/Header';
import Question from './components/Question'

export default {
  name: 'App',
  components: {
    Header,
    Question
  },

  data(){
    return {
      questions: [],
      index: 0,
      correctAnswers: 0,
      answers: [],
    }
  },
  methods: {
    next(){
      if(this.index === 10){
        this.index = 0
      }else{
        this.index++
      }
    },

    increment(isCorrect){
      if(isCorrect){
        this.correctAnswers++
      }
    }
  },
  mounted: function(){
    fetch("https://opentdb.com/api.php?amount=10&category=21&type=multiple")
      .then(res => res.json())
      .then(res => {
        this.questions = res.results
      } )
  },
}
</script>

<style>
</style>
