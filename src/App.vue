<template>
  <BottleCloud></BottleCloud>
</template>

<script>
import BottleCloud from './components/BottleCloud.vue'

export default {
  name: 'App',
  components: {
    BottleCloud
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /*text-align: center;*/
  margin: 0;
  /*margin-top: 60px;*/
}
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>
