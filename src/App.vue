<template>
  <div id="app">
    <HelloPrime />
  </div>
</template>

<script>
import HelloPrime from './components/HelloPrime.vue'

export default {
  name: 'App',
  components: {
    HelloPrime
  }
}
</script>

<style>
    #container{
      display: block;
    position: absolute;
    width: 100%;
    height: 100%;
    } 
</style>
