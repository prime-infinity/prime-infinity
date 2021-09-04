<template>
  <div id="app">
    <!--<HelloPrime />-->
    <Main />
  </div>
</template>

<script>
import Main from './components/Main.vue'

export default {
  name: 'App',
  components: {
    Main
  }
}
</script>

<style>
    @font-face {
        font-family: mutablefont;
        src: url(/font.ttf);
    }
    #app{
        font-family: mutablefont;
    }
    #container{
       height: 100vh;
    } 
    #textFloat{
     
        position:fixed;
        top:50%;
        color:white;
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
