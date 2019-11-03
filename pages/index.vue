<template>
  <div class="container mt-4">
    <div class='text-center'>
      <h1>
        AAP quizzes
      </h1>
      <p>Select a quiz from the list below</p>
      <div class="m-2 d-flex flex-wrap justify-content-center">
        <div class="card m-2" v-for="(title, key) in quizzes" v-bind:key="key">
          <div class="card-body">
            <p>{{title}}</p>
            <div class="btn-group" role="group" aria-label="Basic example">
              <nuxt-link :to="{name: 'flashcards', params: { quiz: key } }" class="btn btn-success">Flashcards</nuxt-link>
              <nuxt-link :to="{name: 'quiz', params: { quiz: key } }" class="btn btn-primary">Quiz</nuxt-link>
            </div>
          </div>
        </div>
      </div>
      <!-- <div class="list-group m-4">
        <a href="#" class="list-group-item">
          <div class="d-flex w-100 justify-content-between">
            <p>Road signs</p>
            <div class="btn-group" role="group" aria-label="Basic example">
              <button type="button" class="btn btn-secondary">Left</button>
              <button type="button" class="btn btn-secondary">Middle</button>
              <button type="button" class="btn btn-secondary">Right</button>
            </div>
          </div>
        </a>
        <nuxt-link :to="{name: 'quiz', params: { quiz:'respiratory' } }" class="list-group-item list-group-item-action">Respiratory System</nuxt-link>
        <nuxt-link :to="{name: 'quiz', params: { quiz:'cardiovascular' } }" class="list-group-item list-group-item-action">Cardiovascular System</nuxt-link>
        <nuxt-link :to="{name: 'quiz', params: { quiz:'nervous' } }" class="list-group-item list-group-item-action">Nervous System</nuxt-link>
        <nuxt-link :to="{name: 'quiz', params: { quiz:'roadsigns' } }" class="list-group-item list-group-item-action">Road signs pages 1-4</nuxt-link>
        <nuxt-link :to="{name: 'quiz', params: { quiz:'roadsigns_2' } }" class="list-group-item list-group-item-action">Road signs pages 5-10</nuxt-link>
      </div> -->
    </div>
  </div>
</template>

<script>
export default {
  components: {
  },
  data () {
    return {
      'quizzes': {
        'respiratory': 'Respiratory System',
        'cardiovascular': 'Cardiovascular System',
        'nervous': 'Nervous System',
        'roadsigns': 'Road signs pages 1-4',
        'roadsigns_2': 'Road signs pages 5-10'
      }
    }
  }
}
</script>

<style scoped>
.container {
  border-radius:5px;
  border:1px solid #ccc;
}
</style>
