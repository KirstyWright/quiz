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
      <b-col align-self="center">
        <div class='text-center mt-5 mb-5'>
          <b-container fluid>
            <b-row class='result-row' v-for="question in questions" v-bind:key='question.content'>
              <b-col class='p-2'>
                <img v-if='question.image' class='p-2 rounded mx-auto d-block' style='max-width:200px;max-height:200px;' v-bind:src="question.content" alt="Question alt image">
                <div v-else>{{question.content}}</div>
                <p>{{question.answer}}</p>
              </b-col>
            </b-row>
          </b-container>
        </div>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  components: {
  },
  data () {
    return {
      title: false,
      questions: false
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
      this.questions = quizObject.questions.sort(() => { return 0.5 - Math.random() })
      for (let i = 0; i < this.questions.length; i++) {
        this.questions[i].image = this.questions[i].content.includes('http')
        if (this.questions[i].answers) {
          for (let x = 0; x < this.questions[i].answers.length; x++) {
            if (this.questions[i].answers[x].correct) {
              this.questions[i].answer = this.questions[i].answers[x].content
            }
          }
        }
      }
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
