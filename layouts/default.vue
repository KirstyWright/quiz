<template>
  <div>
    <nuxt />
    <footer class="footer">
      <div class="container">
        <span class="text-muted">Kirsty Wright | <a href='https://kirsty.dev'>kirsty.dev</a> | <a href='https://github.com/kirstywright/quiz'>Project GitHub</a></span>
      </div>
    </footer>
  </div>
</template>

<style>
html {
  position: relative;
  min-height: 100%;
}
body {
  margin-bottom: 60px; /* Margin bottom by footer height */
}
.footer {
  position: absolute;
  bottom: 0;
  width: 100%;
  height: 60px; /* Set the fixed height of the footer here */
  line-height: 60px; /* Vertically center the text there */
  background-color: #f5f5f5;
}

*,
*:before,
*:after {
  box-sizing: border-box;
  margin: 0;
}
</style>
