<template>
    <div>
      <breads/>
      <reviews/>
      <cartView/>
    </div>
</template>
  <script>

  import cartView from './components/cartView.vue'
  import Breads from './components/Breads.vue'
  import reviews from './components/reviews.vue'
  
  export default {
    name: 'App',
    components: {
        Breads,
        reviews,
        cartView
    }
  }
  </script>
  
  <style>
  
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    background:linear-gradient(
          rgba(0, 0, 0, 0.523), 
          rgba(0, 0, 0, 0.539)
        ), url("assets/background.jpg");
    background-size: cover;
    margin: 0;
    padding:0;
    background-attachment: fixed;
  }
  
  </style>