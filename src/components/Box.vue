<template>
  <div class="qbox-container col-10 offset-1">
    <b-jumbotron>
      <template slot="lead">{{currentQuestion.question}}</template>
      <b-list-group>
        <b-list-group-item v-for="(ans,idx) in answers" :key="idx"
         @click="selectAnswer(idx)"
         :class="[selectedIdx === idx? 'selected' : '']">{{ans}}</b-list-group-item>
      </b-list-group>
      <b-button variant="success col-lg-2 col-sm-12" @click="this.decrementIdx" :disabled='currentIndex===0'>Back</b-button>
      <b-button variant="primary col-lg-2 col-sm-12" :disabled="selectedIdx===null">Submit</b-button>
      <b-button variant="success col-lg-2 col-sm-12" @click="this.incrementIdx" :disabled='totalQuestions===currentIndex+1'>Next</b-button>
    </b-jumbotron>
  </div>
</template>
<script>
export default {
  props: {
    currentQuestion: Object,
    currentIndex: Number,
    totalQuestions:Number,
    incrementIdx: Function,
    decrementIdx: Function
  },
  data(){
      return {
          selectedIdx:null,
      }
  },
  methods:{
      selectAnswer(idx){
          this.selectedIdx = idx;
      }
  },
  watch:{
      currentQuestion(){
          this.selectedIdx=null
      }
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    }
  }
};
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}
.list-group-item:hover {
  background: lightgray;
  cursor: pointer;
}
.btn {
  margin: 3px;
}
.selected {
    background: lightgray;
}
.correct {
    background: lightgreen;
}
.incorrect {
    background: lightcoral;
}
</style>
