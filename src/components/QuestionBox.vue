<template>
  <div class="question-box-container">
    <b-jumbotron header="BootstrapVue" lead="Bootstrap v4 Components for Vue.js 2">
      <template v-slot:lead>
        <span v-html="currentQuestion.question"></span>

      </template>

      <hr class="my-4">
      <b-list-group>
        <b-list-group-item v-for="(answer,index) in shuffledAnswers" :key="index"
        @click="selectAnswer(index)"
        :class="answerClass(index)" v-html="answer"></b-list-group-item>
      </b-list-group>
<!--      <p v-for="(answer,index) in answers" :key="index">-->
<!--        {{answer}}-->
<!--      </p>-->

      <b-button variant="primary"
      @click="submitAnswer"
      :disabled="selectedIndex===null || answered">
        Submit
      </b-button>
      <b-button variant="success" href="#" @click="next">Next</b-button>
    </b-jumbotron>
  </div>
</template>
<script>
import _ from 'lodash'
export default {
  props:{
    currentQuestion:Object,
    next:Function,
    increment:Function,
  },
  data(){
    return {
      selectedIndex:null,
      correctIndex:null,
      shuffledAnswers:[],
      answered:false,
    }
  },
  watch: {
    currentQuestion:{
      immediate:true,
      handler(){
      this.selectedIndex=null;
      this.answered=false;
      this.shuffleAnswers();
      }
    }
  },
  methods:{
    selectAnswer(index){
      this.selectedIndex=index;
      this.correctIndex= this.shuffledAnswers.indexOf(this.answers[3],0);
      console.log(this.correctIndex);
    },
    shuffleAnswers(){
      let answers=[...this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer]
      this.shuffledAnswers=_.shuffle(answers);
    },
    submitAnswer(){
      let isCorrect=false;
      if (this.selectedIndex===this.correctIndex){
        isCorrect=true;
      }
      this.increment(isCorrect);
      this.answered=true;
    },
    answerClass(index){
      let answerClass='';
      if (!this.answered && this.selectedIndex === index){
        answerClass='selected';
      }
      else if (this.answered && this.correctIndex=== index){
        answerClass='correct';
      }
      else if (this.answered && this.selectedIndex === index && this.correctIndex!==index){
        answerClass='incorrect';
      }
      return answerClass;
    }
  },
  computed:{
    answers(){
      let answers=[...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    },
  }
}
</script>
<style scoped>
  .list-group{
    margin-bottom: 15px;
  }
  .btn{
    margin: 0 5px;
  }
  .list-group-item:hover{
    background: #EEE;
    cursor: pointer;
  }
  .selected{
    background: lightblue;
    color: white;
  }
  .correct{
    background: lightgreen;
    color: white;
  }
  .incorrect{
    background: lightcoral;
    color: white;
  }
</style>