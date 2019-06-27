<template>
  <div class="qbox-container col-10 offset-1">
    <b-jumbotron>
      <template slot="lead">{{currentQuestion.question}}</template>
      <b-list-group>
        <b-list-group-item
          v-for="(ans,idx) in answers"
          :key="idx"
          @click.prevent="isAnswered? '':selectAnswer(idx)"
          :class="[selectedClass(idx)]"
        >{{ans}}</b-list-group-item>
      </b-list-group>
      <b-button
        variant="success col-lg-2 col-sm-12"
        @click="this.decrementIdx"
        :disabled="currentIndex===0"
      >Back</b-button>
      <b-button
        variant="primary col-lg-2 col-sm-12"
        :disabled="this.selectedIdx===null || isAnswered"
        @click="isAnswered? '': submitAnswer()"
      >Submit</b-button>
      <b-button
        variant="success col-lg-2 col-sm-12"
        @click="this.incrementIdx"
        :disabled="totalQuestions===currentIndex+1"
      >Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";

export default {
  props: {
    currentQuestion: Object,
    currentIndex: Number,
    totalQuestions: Number,
    incrementIdx: Function,
    decrementIdx: Function,
    Answered: Boolean,
    setAnsweredState: Function,
    setSelectedAnswer: Function,
    selectedAnswerIdx: String
  },
  data() {
    return {
      selectedIdx: null,
      correctAnswerIndex: null,
      isAnswered: false
    };
  },
  methods: {
    selectAnswer(idx) {
      this.selectedIdx = idx;
      this._props.setSelectedAnswer(
        this._props.currentIndex,
        this.answers[idx]
      );
    },
    selectedClass(idx) {
      if (this.isAnswered) {
        if (this.correctAnswerIndex === idx) return "correct";
        if (this.selectedIdx === idx && idx !== this.correctAnswerIndex)
          return "incorrect";
        return "";
      } else {
        return this.selectedIdx === idx ? "selected" : "";
      }
    },
    submitAnswer() {
      this.isAnswered = true;
      this._props.setAnsweredState(this._props.currentIndex, true);
    }
  },
  computed: {
    answers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer
      ];
      answers = _.shuffle(answers);
      this.correctAnswerIndex = answers.indexOf(
        this.currentQuestion.correct_answer
      );
      this.selectedIdx = answers.indexOf(this._props.selectedAnswerIdx);
      return answers;
    }
  },
  watch: {
    currentIndex: {
      immediate: true,
      handler: function() {
        this.isAnswered = this._props.Answered;
        console.log("s");
      }
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
  background: lightblue;
}
.correct {
  background: lightgreen;
}
.incorrect {
  background: lightcoral;
}
</style>
