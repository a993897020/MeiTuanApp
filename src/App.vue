<template>
  <div id="app">
    <app-header :poiInfo="poiInfo"></app-header>
    <app-nav></app-nav>
   <router-view :poiInfo="poiInfo"></router-view>
    
  </div>
</template>

<script>
import Header from './components/header/Header'
import Nav from './components/nav/Nav'
import axios from 'axios'

export default {
  name: 'App',
  data(){
    return {
      poiInfo:{}
    }
  },
  components: {
   'app-header':Header,
   'app-nav':Nav,
  },
  created(){
    axios.get('api/goods')
    .then(res=>{
      // console.log(res.data)
      if(res.data.code==0){
        this.poiInfo=res.data.data.poi_info 
        // console.log(this.poiInfo)
      }
    })
  }
}
</script>

<style>

</style>
