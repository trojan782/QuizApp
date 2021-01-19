<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template #lead>
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4" />

      <!-- just print what you have from the shuffledAnswers not the initial answers coming from the endpoint -->
      <b-list-group v-for="(answer, index) in shuffledAnswers" :key="index">
        <b-list-group-item
          @click="selectAnswer(index)"
          :class="[selectedIndex === index ? 'selected' : '']"
          >{{ answer }}</b-list-group-item
        >
      </b-list-group>

      <b-button variant="primary">Submit</b-button>
      <!-- Then for this next props ur passing its fine but try to learn how to emit events but what ur doing already is fine.
        example: check the methods for the commented function
        <b-button variant="success" @click="nextFunc">Next</b-button>
       -->
      <b-button variant="success" @click="next">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import ash from 'lodash'
export default {
  props: {
    currentQuestion: Object,
    next: Function, // if ur using the emit event then no need to recieve this props function heres
  },
  data() {
    return {
      selectedIndex: null,
      shuffledAnswers: [],
    };
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    },
  },
  watch: {
    currentQuestion() {
      this.selectedIndex = null;
      this.shuffleAnswers();
    },
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
      console.log(index);
    },
    shuffleAnswers() {
      // console.log(this.shuffledAnswers);
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ]
      // console.log(answers);
      // here just shuffle the array since the data type of the answers variable is an array
      // shuffle works well with arrays but i havent tried with objects.
      this.shuffledAnswers = ash.shuffle(answers);
      // console.log(this.shuffledAnswers);
    },
    // the function will emit an event in your parent component app.js
    // nextFunc() {
    //   this.$emit('nextQuestion');
    // }
  },
    mounted() {
        this.shuffleAnswers();
    },
 
};
</script>

<style scoped>
.list-group-item {
  margin-bottom: 10px;
}
.list-group-item:hover {
  background-color: dodgerblue;
  color: white;
  cursor: pointer;
}
.btn {
  margin: 0 5px;
}
.selected {
  background-color: #e2e2;
}
.correct {
  background-color: lime;
}
.incorrect {
  background-color: red;
}
</style>
