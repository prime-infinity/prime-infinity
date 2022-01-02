<template>
  <div id="app">
    <HelloPrime />
    <!--<Main />-->
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
    /*@font-face {
      font-family: mutablefont;
      src: url('/font.ttf');
    }*/
    #app{
      font-family: 'Major Mono Display', monospace;
    }
    #container{
       height: 100vh;
    } 
    #textFloat{
     
        position:fixed;
        top:50%;
        /*color:white;*/
        left: 50%;
        transform: translate(-50%, -50%);
        text-align: center;
        width:100%;
    }
    .appearOne,
    .appearTwo{
        opacity: 0;
    }

</style>
