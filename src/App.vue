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
    @font-face {
        font-family: mutablefont;
        src: url(/font.ttf);
    }
    #app{
        font-family: mutablefont;
    }
    #container{
      background: black url(/img/stars.png) repeat;
      position: absolute;
      width:100%;
      height: 100%;
      display: block;
      z-index: 0;
    } 
    .twinkling {
      width: 1000px;
      height: 100%;
      background: transparent url(/img/twinkling.png) repeat;
      background-size: 1000px 1000px;
      position: absolute;
      right: 0;
      top: 0;
      bottom: 0;
      z-index: 2;
      -moz-animation: move-background 70s linear infinite;
      -ms-animation: move-background 70s linear infinite;
      -o-animation: move-background 70s linear infinite;
      -webkit-animation: move-background 70s linear infinite;
      animation: move-background 70s linear infinite;
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

    @keyframes move-background {
        from {
            -webkit-transform: translate3d(0px, 0px, 0px);
        }
        to {
            -webkit-transform: translate3d(1000px, 0px, 0px);
        }
    }

    @-webkit-keyframes move-background {
        from {
            -webkit-transform: translate3d(0px, 0px, 0px);
        }
        to {
            -webkit-transform: translate3d(1000px, 0px, 0px);
        }
    }

    @-moz-keyframes move-background {
        from {
            -webkit-transform: translate3d(0px, 0px, 0px);
        }
        to {
            -webkit-transform: translate3d(1000px, 0px, 0px);
        }
    }

    @-webkit-keyframes move-background {
        from {
            -webkit-transform: translate3d(0px, 0px, 0px);
        }
        to {
            -webkit-transform: translate3d(1000px, 0px, 0px);
        }
    }
</style>
