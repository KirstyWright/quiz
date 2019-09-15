<template>
  <div class="container mt-4">
    <div class='text-center'>
      <h1>
        A &amp; P quizzes
      </h1>
      <p>Select a quiz from the list below</p>
      <div class="list-group m-4">
        <nuxt-link :to="{name: 'quiz', params: { quiz:'demo' } }" class="list-group-item list-group-item-action">Demo</nuxt-link>
        <nuxt-link :to="{name: 'quiz', params: { quiz:'respiratory' } }" class="list-group-item list-group-item-action">Respiratory System</nuxt-link>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  components: {
  }
}
</script>

<style scoped>
.container {
  border-radius:5px;
  border:1px solid #ccc;
}
</style>
