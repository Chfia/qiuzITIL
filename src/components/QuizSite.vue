<template>
    <div>
      <h1>{{ currentQuestion.questionText }}</h1>
      <ul>
        <li
          v-for="(answer, index) in shuffledAnswers"
          :key="index"
          @click="checkAnswer(index)"
          :class="{ correct: isCorrectAnswer(index), incorrect: isIncorrectAnswer(index) }"
        >
          {{ answer.text }}
        </li>
      </ul>
      <button @click="nextQuestion">Next</button>
    </div>
  </template>
  
  <script>
  import questionsData from "./questions.json";
  
  export default {
    data() {
      return {
        questions: [],
        currentQuestionIndex: 0,
        answeredCorrectly: [],
      };
    },
    computed: {
      currentQuestion() {
        return this.questions[this.currentQuestionIndex];
      },
      shuffledAnswers() {
        const answers = [...this.currentQuestion.answers];
        for (let i = answers.length - 1; i > 0; i--) {
          const j = Math.floor(Math.random() * (i + 1));
          [answers[i], answers[j]] = [answers[j], answers[i]];
        }
        return answers;
      },
    },
    methods: {
      isCorrectAnswer(index) {
        return (
          this.answeredCorrectly.includes(this.currentQuestion.id) &&
          this.shuffledAnswers[index].correct
        );
      },
      isIncorrectAnswer(index) {
        return (
          !this.answeredCorrectly.includes(this.currentQuestion.id) &&
          index === this.currentQuestion.userAnswer
        );
      },
      checkAnswer(index) {
        if (!this.answeredCorrectly.includes(this.currentQuestion.id)) {
          if (this.shuffledAnswers[index].correct) {
            this.answeredCorrectly.push(this.currentQuestion.id);
          }
          this.currentQuestion.userAnswer = index;
        }
      },
      nextQuestion() {
        if (this.answeredCorrectly.length === this.questions.length) {
          alert("Gratulacje! Zakończyłeś grę.");
          return;
        }
        let unansweredQuestions = this.questions.filter(
          (question) => !this.answeredCorrectly.includes(question.id)
        );
        if (unansweredQuestions.length === 0) {
          alert("Brak dostępnych pytań. Zakończono grę.");
          return;
        }
  
        const randomIndex = Math.floor(Math.random() * unansweredQuestions.length);
        const nextQuestion = unansweredQuestions[randomIndex];
        this.currentQuestionIndex = this.questions.indexOf(nextQuestion);
      },
      loadQuestions() {
        this.questions = questionsData.questions;
        this.shuffleQuestions();
      },
      shuffleQuestions() {
        for (let i = this.questions.length - 1; i > 0; i--) {
          const j = Math.floor(Math.random() * (i + 1));
          [this.questions[i], this.questions[j]] = [this.questions[j], this.questions[i]];
        }
      },
    },
    created() {
      this.loadQuestions();
    },
  };
  </script>
  
  <style scoped>
  .correct {
    background-color: green;
  }
  .incorrect {
    background-color: red;
  }
  </style>
  