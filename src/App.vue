<template>
  <div id="app">
    <!-- <img src="./assets/logo.png"> -->
    <leftNav></leftNav>
    <div class="main">
      <router-view></router-view>
    </div>
    <!-- <router-view/> -->
  </div>
</template>

<script>
import leftNav from '@/components/conmon/leftNav'
export default {
  name: 'App',
  components:{
    leftNav
  }
}
</script>

<style>
#app {
  font-family: 'Microsoft YaHei', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /* margin-top: 60px; */
}
.main{
  float: left;
  width: 95%;
  background-color: #EFF2F7;
  height: 100%;
  overflow: hidden;
}
</style>



