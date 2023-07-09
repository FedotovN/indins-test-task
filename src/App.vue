<template>
  <div class="wrapper">
    <main-page></main-page>
  </div>
</template>

<script>
import MainPage from './components/MainPage.vue';
export default {
  name: 'App',
  components: {
    MainPage
  },
  data: () => ({})
}
</script>

<style lang="scss">
  * {
    padding: 0;
    margin: 0;
  }
  .wrapper {
    height: 100vh;
    width: 100%;
  }
</style>
