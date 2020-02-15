<template>
  <div id="app">
    <header-app :numCorrect="numCorrect" :numTotal="numTotal"></header-app>
    <b-container>
      <b-row>
        <b-col sm="6" offset="3">
          <question
            v-if="questionsList.length"
            :next="next"
            :increment="increment"
            :currentQuestion="questionsList[index]"
          ></question>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import HeaderApp from "./components/Header";
import question from "./components/QuestionBox";
export default {
  name: "App",
  data() {
    return {
      questionsList: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0
    };
  },
  components: {
    HeaderApp,
    question
  },
  methods: {
    next() {
      this.index++;
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++;
      }
      this.numTotal++;
    }
  },
  mounted() {
    fetch("https://opentdb.com/api.php?amount=10&category=27&type=multiple", {
      method: "get"
    })
      .then(response => {
        return response.json();
      })
      .then(jsonData => {
        this.questionsList = jsonData.results;
      });
  }
};
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
