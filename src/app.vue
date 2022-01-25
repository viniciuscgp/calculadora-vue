<template>
  <div id="app">
     <Calculator :kpress="kpress" />
  </div>
</template>

<script>
   import Calculator from "./main/Calculator.vue"

   export default {
      components: { Calculator },
      data() {
         return {
            kpress: ''
         }
      },
      mounted() {
         document.addEventListener('keypress', this.onkpress);
      },
      methods: {
         onkpress(e) {
            this.kpress = e.key;
         }
      }

   }
</script>

<style>
   * {
      font-family: 'Roboto Mono', sans-serif;
   }

   body {
      margin: 0px;
   }

   #app {
      display: flex;
      flex-direction: column;
      height: 100vh;
      justify-content: space-around; 
      align-items: center;
      text-align: center;
      color: white;
      background-image: linear-gradient(
         50deg,
         hsl(228deg 89% 25%) 0%,
         hsl(229deg 63% 34%) 44%,
         hsl(228deg 51% 42%) 99%
      );
   }

</style>