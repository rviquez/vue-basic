<template>
  <div>
    <v-card>
      <v-responsive :aspect-ratio="16/9">
        <v-card>
          <v-card-text>
            {{ currentQuestion.question }}
            <v-spacer></v-spacer>
          </v-card-text>
        </v-card>
        <template>
          <v-item-group>
            <v-container grid-list-md>
              <v-layout wrap>
                <v-flex
                  v-for="(answer, index) in shuffledAnswers" :key="index"
                  xs12
                  md4
                  
                >
                  <v-item class="item">
                    <v-card
                    class="d-flex align-center"
                    dark
                    height="50"
                    @click="selectAnswer(index)"
                    :class="answerClass(index)"
                    >
                      <v-scroll-y-transition>
                        <div
                          class="text-xs-center"
                        >
                          {{ answer }}
                        </div>
                      </v-scroll-y-transition>
                    </v-card>
                  </v-item>
                </v-flex>
              </v-layout>
            </v-container>
          </v-item-group>
        </template>
        <v-btn 
          @click="submitAnswer"
          :disabled="selectedIndex === null || answered" 
          color="primary" 
          raised>
          Submit
        </v-btn>
        <v-btn @click="next" color="success" raised>
          Next
        </v-btn>
      </v-responsive>
    </v-card>
  </div>
</template>

<script>
import _ from 'lodash';
export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false
    }
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.answered = false;
        this.shuffleAnswers();
      }
    }
  },
  methods: {
    selectAnswer(index){
      this.selectedIndex = index;
      console.log(index);
    },
    submitAnswer(){
      let isCorrect = false;

      if(this.selectedIndex === this.correctIndex){
        isCorrect = true;
      }
      this.answered = true;
      this.increment(isCorrect);
    },
    shuffleAnswers(){
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);
    },
    answerClass(index){
      let answerClass = '';

      if(!this.answered && this.selectedIndex === index){
        answerClass = 'primary';
      }else if(this.answered && this.correctIndex === index){
        answerClass = 'success';
      } else if(this.answered && 
        this.selectedIndex === index && 
        this.correctIndex !== index){
        answerClass = 'error';
      }
      return answerClass;
    }
  }
}
</script>

<style scoped>
.item:hover{
  background-color: #808080;
}
.correct {
  background-color: green;
}
.incorrect {
  background-color: red;
}
</style>