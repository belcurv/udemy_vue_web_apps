<template>
  <div id="app">
    <h3>Starbase Router</h3>
    <h4>
      <router-link class="link" to="/data/people">Meet the Characters</router-link> |
      <router-link class="link" to="/data/planets">Explore the Planets</router-link> |
      <router-link class="link" to="/data/vehicles">Vehicles</router-link>
    </h4>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
}
</script>

<style lang="css">
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: white;
    margin-top: 60px;
  }

  .link, .link:hover, .link:visited, .link:active {
    color: #4fc08d;
  }
</style>
