<template>
  <div class="question-box-container">
    <b-jumbotron lead="Bootstrap v4 Components for Vue.js 2">
      <template #lead>
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4" />

      <!-- <p>
        {{ answer }}
      </p> -->
      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in answers"
          :key="index"
          @click="selectAnswer(index)"
          :class="answerClass(index)"
          >{{ answer }}</b-list-group-item
        >
      </b-list-group>

      <b-button
        variant="primary"
        href="#"
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered"
        >Submit
      </b-button>
      <b-button variant="success" href="#" @click="next">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";
export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function,
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.shuffleAnswers();
        this.answered = false;
      },
    },
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false,
    };
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    },
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
      console.log(this.selectedIndex);
    },
    shuffleAnswers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      this.shuffledAnswers = _.shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(
        this.currentQuestion.correct_answer
      );
    },
    submitAnswer() {
      let isCorrect = false;
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.answered = true;
      this.increment(isCorrect);
    },
    answerClass(index){
        let answerClass = ''
        if(this.selectedIndex === index && !this.answered){answerClass='selected'}
        else if(this.answered && this.correctIndex === index){answerClass='correct'}
        else if(this.answered && this.selectedIndex === index && this.selectedIndex !== this.correctIndex){answerClass='incorrect'}

        return answerClass
    }
  },
};
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}
.list-group-item:hover {
  background: #eee;
  cursor: pointer;
}
.selected {
  background-color: lightblue;
}

.correct {
  background-color: lightgreen;
}

.incorrect {
  background-color: red;
}
.btn {
  margin: 0 5px;
}
</style>