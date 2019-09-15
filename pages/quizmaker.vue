<template lang="html">
  <b-container>
    <div class="">
      <h1>Quiz maker</h1>
      <b-form-input class='mt-1' v-model="question" placeholder="Question"></b-form-input>
      <b-form-input class='mt-1' v-model="answer1" placeholder="Correct answer"></b-form-input>
      <b-form-input class='mt-1' v-model="answer2" placeholder="Incorrect answer"></b-form-input>
      <b-form-input class='mt-1' v-model="answer3" placeholder="Incorrect answer"></b-form-input>
      <b-form-input class='mt-1' v-model="answer4" placeholder="Incorrect answer"></b-form-input>
      <b-button class='mt-1' variant="success" @click='addQuestion'>Add question</b-button>
      <textarea class='mt-1' v-model="jsonString"></textarea>
    </div>
  </b-container>
</template>

<script>
export default {
  data () {
    return {
      jsonString: '',
      question: '',
      answer1: '',
      answer2: '',
      answer3: '',
      answer4: '',
      questions: []
    }
  },
  methods: {
    addQuestion () {
      const question = {
        content: this.question,
        answers: [
          {
            content: this.answer1,
            correct: true
          },
          {
            content: this.answer2,
            correct: false
          },
          {
            content: this.answer3,
            correct: false
          },
          {
            content: this.answer4,
            correct: false
          }
        ]
      }
      this.questions.push(question)
      this.jsonString = JSON.stringify(this.questions)
    }
  }
}
</script>

<style lang="css" scoped>
</style>
