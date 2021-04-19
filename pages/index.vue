<template>
  <div class="container">
    <div>
      <Logo />
      <h1 class="title">
        strapi-tester-frontend
      </h1>
      <div class="thing">
        I am the thing
      </div>
      <div class="links">
        <a
          href="https://nuxtjs.org/"
          target="_blank"
          rel="noopener noreferrer"
          class="button--green"
        >
          Documentation
        </a>
        <a
          href="https://github.com/nuxt/nuxt.js"
          target="_blank"
          rel="noopener noreferrer"
          class="button--grey"
        >
          GitHub
        </a>
        <nuxt-link to="/book">
          Book paddle
        </nuxt-link>
      </div>
    </div>
  </div>
</template>

<script>
export default {}
</script>
