<template>
  <b-container class='mt-4 quiz-container'>
    <div class="d-flex justify-content-center mb-5 mt-5" v-if='!title'>
      <b-spinner label="Loading..."></b-spinner>
    </div>
    <b-row v-if='title'>
      <b-col align-self="center">
        <h1 class='text-center'>
          {{this.title}}
        </h1>
      </b-col>
    </b-row>
    <b-row v-if='title'>
      <b-col align-self="center" v-if="question">
        <Question v-if="question.answers" :key="question.content" :name="question.content" :answers="question.answers" @answered="nextQuestion"/>
        <WrittenQuestion v-if="question.answer" :key="question.content" :name="question.content" :answer="question.answer" @answered="nextQuestion"/>
      </b-col>
      <b-col align-self="center" v-if="!question && this.remainingQuestions.length == 0">
        <div class='text-center mt-5 mb-5'>
          You have finished the quiz
          <nuxt-link to="/">Select another quiz</nuxt-link>
        </div>
      </b-col>
    </b-row>
    <b-row class='mt-4' v-if='title'>
      <b-col>
        <p class='text-center'>
          <span class='text-success'>Correct: {{this.correctCount}}</span>&nbsp;|&nbsp;
          <span class='text-danger'>Incorrect: {{this.incorrectCount}}</span>&nbsp;|&nbsp;
          <span class='text-secondary'>Remaining: {{this.remainingQuestions.length}}</span>&nbsp;|&nbsp;
        </p>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import Question from '../components/Question.vue'
import WrittenQuestion from '../components/WrittenQuestion.vue'
export default {
  components: {
    Question,
    WrittenQuestion
  },
  data () {
    return {
      title: false,
      question: false,
      askedQuestions: [],
      remainingQuestions: [],
      correctCount: 0,
      incorrectCount: 0
    }
  },
  mounted () {
    if (typeof this.$route.params.quiz === 'undefined') {
      this.$router.push('/')
    }
    this.buildQuiz(this.$route.params.quiz)
  },
  methods: {
    async buildQuiz (param) {
      const quizObject = await this.$axios.$get('/json/' + param + '.json').catch(() => { this.$router.push('/') })
      this.title = quizObject.name
      this.remainingQuestions = quizObject.questions.sort(() => { return 0.5 - Math.random() })
      this.nextQuestion()
    },
    nextQuestion (value) {
      if (this.question && value) {
        this.question.answered = value
        this.askedQuestions.push(this.question)
        if (value.correct) {
          this.correctCount++
        } else {
          this.incorrectCount++
        }
      }
      this.question = this.remainingQuestions.pop()
    }
  }
}
</script>

<style>
.quiz-container {
  border-radius:5px;
  border:1px solid #ccc;
}
</style>
