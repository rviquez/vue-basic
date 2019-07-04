<template>
  <v-app dark>
    <v-container grid-list-md text-xs-center>
      <Header 
        :numCorrect="numCorrect"
        :numTotal="numTotal"
      />
      <v-layout row wrap>
        <v-flex v-flex xs6 offset-xs3 offset-md2 offset-lg5>
          <QuestionBox
            v-if="questions.length" 
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
          />
        </v-flex>
      </v-layout>
    </v-container>
  </v-app>
</template>

<script>
import Header from './components/Header'
import QuestionBox from './components/QuestionBox'

export default {
  name: 'App',
  components: {
    Header,
    QuestionBox
  },
  data () {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0
    }
  },
  methods: {
    next(){
      this.index++
    },
    increment(isCorrect){
      if(isCorrect){
        this.numCorrect++;
      }
      this.numTotal++;
    }
  },
  mounted: function(){
    fetch('https://opentdb.com/api.php?amount=10&category=27&type=multiple', {
      method: 'get'
    })
    .then((response) =>{ 
      return response.json();
    })
    .then((jsonData)=>{
      this.questions = jsonData.results;     
    })
  }
}
</script>