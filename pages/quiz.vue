<template>
  <b-container class='mt-4 quiz-container'>
    <b-row>
      <b-col align-self="center">
        <h1 class='text-center'>
          {{this.title}}
        </h1>
      </b-col>
    </b-row>
    <b-row>
      <b-col align-self="center" v-if="question">
        <Question :key="question.content" :name="question.content" :answers="question.answers" @answered="nextQuestion"/>
      </b-col>
      <b-col align-self="center" v-if="!question && this.remainingQuestions.length == 0">
        <div class='text-center mt-5 mb-5'>
          You have finished the quiz
          <nuxt-link to="/">Select another quiz</nuxt-link>
        </div>
      </b-col>
    </b-row>
    <b-row class='mt-4'>
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
export default {
  components: {
    Question
  },
  data () {
    return {
      title: '',
      question: false,
      askedQuestions: [],
      remainingQuestions: [],
      correctCount: 0,
      incorrectCount: 0
    }
  },
  mounted () {
    console.log(this.$route)
    // if (typeof this.$route.query.q === 'undefined') {
    //   this.$router.push('/')
    // }
    this.buildQuiz(this.$route.query.q)
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
