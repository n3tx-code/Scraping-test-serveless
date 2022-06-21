<template>
  <div id="app">
    <h1>Scraping test</h1>
    <ScrappingWrapper></ScrappingWrapper>
  </div>
</template>

<script>
import ScrappingWrapper from "@/components/ScrapingWrapper";

export default {
  name: 'App',
  components: {
    ScrappingWrapper,
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Ubuntu:ital,wght@0,300;0,400;0,700;1,300;1,400&display=swap');

body, html {
  font-family: 'Ubuntu', sans-serif;
  padding: 25px;
}

h1 {
  text-align: center;
}
</style>
