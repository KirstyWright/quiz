<template lang="html">
  <div>
    <div class="text-center">
      <h4>{{name}}</h4>
    </div>
    <img v-if='url' class='p-2 rounded mx-auto d-block' style='max-width:200px;max-height:200px;' v-bind:src="url" alt="Question alt image">
    <div class="text-center">
        <p>{{tooltip}}</p>
    </div>
    <div class='d-flex flex-row flex-wrap justify-content-center pt-2'>
        <div class="form-group">
            <input
            id="submission-input"
            type="text"
            class="form-control"
            v-model="submission"
            placeholder="Enter answer"
            v-bind:class='{ "answer--incorrect": (this.correct === false), "answer--correct": this.correct }'
            v-on:keyup.enter="submit"
            >
            <small class="text-center form-text text-muted">Hit enter to submit your answer</small>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    name: String,
    answer: String,
    url: String
  },
  data () {
    return {
      tooltip: '',
      submission: '',
      correct: null
    }
  },
  mounted () {
    document.getElementById('submission-input').focus()
    if (this.name.includes('http')) {
      this.url = this.name
      this.name = ''
    }
  },
  methods: {
    submit () {
      if (this.submission.toLowerCase().trim() === this.answer.toLowerCase().trim()) {
        this.tooltip = 'Correct!'
        this.correct = true
      } else {
        this.tooltip = 'Wrong! The correct answer is: ' + this.answer
        this.correct = false
      }
      setTimeout(() => {
        this.$emit('answered', { correct: this.correct })
      }, 1000)
    }
  }
}
</script>

<style lang="css" scoped>

.answer:hover {
  background-color:#007bff;
  transition:.5s;
}
.answer.answer--correct:hover,
.answer.answer--incorrect:hover {
  background-color:inherit;
}
.answer--correct {
  border-color:#28a745 !important;
  transition:.5s;
}
.answer--incorrect {
  border-color:#dc3545 !important;
  transition:.5s;
}
</style>
