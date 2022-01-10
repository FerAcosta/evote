<template>
  <div id="app">
   <img alt="CS Logo" src="./assets/logo.png" width="350" height="80">
      
        <br>-<p><router-link to="/">Inicio</router-link>&nbsp;&nbsp;&nbsp;         
        <!-- <router-link to="/castBallot">Cast Ballot</router-link>&nbsp; -->
        <router-link to="/getCurrentStanding">Resultados de Votación</router-link>&nbsp;</p>

      <router-view></router-view>
    </div>
</template>

<script>

export default {
  name: 'app'  
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 40px;
}
.chart-wrapper {
  width: 100%;
  height: 700px;
}
.echarts {
  width: 100%;
  height: 100%;
}

#loader {
  position:fixed;
  padding-top: 250px;
}

.chart-wrapper {
  position:fixed;
  padding-left:32%;
}
</style>
