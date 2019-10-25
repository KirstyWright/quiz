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
      <b-col align-self="center" v-if="question" :key="questionCount">
        <Question v-if="question.answers" :key="question.content" :name="question.content" :answers="question.answers" @answered="nextQuestion"/>
        <WrittenQuestion v-if="question.answer" :key="question.content" :name="question.content" :answer="question.answer" @answered="nextQuestion"/>
      </b-col>
      <b-col align-self="center" v-if="!question && this.remainingQuestions.length == 0">
        <div class='text-center mt-5 mb-5'>
          You have finished the quiz
          <nuxt-link to="/">Select another quiz</nuxt-link>
          <p class='text-center'>
            <span class='text-success'>First time correct: {{this.correctCount}}</span>&nbsp;|&nbsp;
            <span class='text-danger'>Incorrect total: {{this.incorrectCount}}</span>&nbsp;|&nbsp;
          </p>
          <b-container fluid>
            <b-row class='result-row'>
              <b-col class='p-2'>
                <h4>Asked questions</h4>
              </b-col>
              <b-col class='p-2'>
                <h4>Your answers</h4>
              </b-col>
            </b-row>
            <b-row class='result-row' v-for="askedQuestion in askedQuestions" v-bind:key='askedQuestion.content'>
              <b-col class='p-2'>
                <img v-if='askedQuestion.image' class='p-2 rounded mx-auto d-block' style='max-width:200px;max-height:200px;' v-bind:src="askedQuestion.content" alt="Question alt image">
                <div v-else>{{askedQuestion.content}}</div>
              </b-col>
              <b-col class='p-2'>
                <div v-for="(askedAnswer, answerIndex)  in askedQuestion.submissions" v-bind:key="answerIndex">
                  {{answerIndex + 1}}:
                  <span v-bind:class='{ "asked-answer--incorrect": (answerIndex !== (askedQuestion.submissions.length - 1)), "asked-answer--correct": (answerIndex === (askedQuestion.submissions.length - 1)) }'>{{askedAnswer}}</span>
                </div>
              </b-col>
            </b-row>
          </b-container>
        </div>
      </b-col>
    </b-row>
    <b-row class='mt-4' v-if='title'>
      <b-col v-if="question">
        <p class='text-center'>
          <span class='text-success'>First time correct: {{this.correctCount}}</span>&nbsp;|&nbsp;
          <span class='text-danger'>Incorrect: {{this.incorrectCount}}</span>&nbsp;|&nbsp;
          <span class='text-secondary'>Remaining: {{this.remainingQuestions.length}}</span>&nbsp;|&nbsp;
        </p>
        <p>
          <small class='text-center' v-if="roundNo > 1">Round number {{roundNo}}</small>
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
      askedQuestions: {},
      remainingQuestions: [],
      nextRoundQuestions: [],
      correctCount: 0,
      incorrectCount: 0,
      roundNo: 0,
      questionCount: 0
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
      this.roundNo = 1
      this.questionCount = 1
      this.nextQuestion()
    },
    nextQuestion (value) {
      if (this.question && value) {
        if (!(this.question.content in this.askedQuestions)) {
          this.askedQuestions[this.question.content] = this.question
          this.askedQuestions[this.question.content].image = this.question.content.includes('http')
          this.askedQuestions[this.question.content].submissions = []
        }
        this.askedQuestions[this.question.content].submissions.push(value.submission)

        if (value.correct) {
          if (this.roundNo === 1) { // only count round 1 correct answers
            this.correctCount++
          }
        } else {
          this.incorrectCount++
          this.nextRoundQuestions.push(this.question) // keep asking till get correct
        }
      }

      // end of round
      if (this.remainingQuestions.length === 0) {
        this.remainingQuestions = this.nextRoundQuestions
        this.nextRoundQuestions = []
        this.roundNo++
      }
      this.question = this.remainingQuestions.pop()
      this.questionCount++
      this.$forceUpdate()
    }
  }
}
</script>

<style>
.quiz-container {
  border-radius:5px;
  border:1px solid #ccc;
}
.result-row > .col:first-child{
  border-right: 1px solid #ccc;
}
.result-row {
  border-bottom: 1px solid #ccc
}
.asked-answer--incorrect {
  color:#dc3545;
}
.asked-answer--correct {
  color:#28a745;
}
</style>
