<template>
  <div id="app">
  <Header
    v-bind:totalAnswered="totalAnswered"
    v-bind:totalCorrect="totalCorrect" 
  />
  <hr>
  <b-container class="bv-example-row">
    <b-row>
      <b-col sm='6' offset='3'>
          <questionBox 
          v-if="questions.length"
          v-bind:currentQuestion='questions[index]'
          :next="next"
          :increaseCount="increaseCount"
           />
      </b-col>
    </b-row>
  </b-container>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import questionBox from './components/questionBox.vue'
import axios from 'axios'

export default {
  name: 'App',
  components: {
    questionBox,
    Header
  },
  data(){
    return {
      questions: [],
      index: 0,
      totalAnswered: 0,
      totalCorrect: 0
    }
  },

  methods: {
    next() {
      console.log(this.index)
      if ( this.index <= this.questions.length - 2 ) {
        this.index++
      } else {
        alert('That is all for now')
      }

    },

    increaseCount(isCorrect) {
      if(isCorrect) {
        this.totalCorrect++
      }
      this.totalAnswered++
    }
  },

  created() {
    axios('https://opentdb.com/api.php?amount=10&category=27&difficulty=medium&type=multiple')
    .then( res => this.questions = res.data.results)
    .catch(err => console.log(err))
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
