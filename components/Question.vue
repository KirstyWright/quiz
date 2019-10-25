<template lang="html">
  <div>
    <div class="text-center">
      <h4>{{name}}</h4>
      <p>{{tooltip}}</p>
    </div>
    <div class='d-flex flex-row flex-wrap justify-content-center pt-2'>
      <div
        class="answer p-2 text-center"
        :key="answer.id"
        v-for="answer in internalAnswers"
        @click="selectAnswer(answer.id)"
        v-bind:class='{ "answer--incorrect": (answer.selected && !answer.correct), "answer--correct": ((answer.selected && answer.correct) || (answer.correct && answered)) }'
      >
        {{answer.content}}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    name: String,
    answers: Array
  },
  data () {
    return {
      internalAnswers: [],
      tooltip: 'Pick one answer',
      answered: false
    }
  },
  mounted () {
    const answers = []
    for (let i = 0; i < this.answers.length; i++) {
      const answer = this.answers[i]
      answer.id = (i + 1)
      answer.selected = false
      answers.push(answer)
    }
    this.internalAnswers = answers.sort(() => { return 0.5 - Math.random() })
  },
  methods: {
    selectAnswer (id) {
      const answer = this.internalAnswers.find(function (answer) {
        return answer.id === id
      })
      answer.selected = true
      answer.submission = answer.content
      if (answer.correct) {
        this.tooltip = 'Congrats'
      } else {
        this.tooltip = 'Uh oh '
      }
      this.answered = true
      setTimeout(() => {
        this.$emit('answered', answer)
      }, 1000)
    }
  }
}
</script>

<style lang="css" scoped>
.answer {
  width: calc(50% - 10px);
  border-radius:10px;
  border:1px solid #ccc;
  margin:5px;
  cursor:pointer;
}

.answer:hover {
  background-color:#007bff;
  transition:.5s;
}
.answer.answer--correct:hover,
.answer.answer--incorrect:hover {
  background-color:inherit;
}
.answer--correct {
  background-color:#28a745 !important;
  transition:.5s;
}
.answer--incorrect {
  background-color:#dc3545 !important;
  transition:.5s;
}
</style>
