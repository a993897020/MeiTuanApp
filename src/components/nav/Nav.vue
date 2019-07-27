<template>
  <div class="nav">
   <router-link to="/goods" class="nav-item">
   点菜
   <i class="line"></i>
   </router-link>
   <router-link to="/ratings" class="nav-item">
   评价
   <i class="line"></i>
   </router-link>
   <router-link to="/seller" class="nav-item">
   商家
   <i class="line"></i>
   </router-link>
  </div>
</template>

<script>
export default {
  data () {
    return {
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.nav{
    width: 100%;
    display: flex;
    height: 40px;
    line-height: 40px;

}
.nav-item{
    position: relative;
    flex:1;
    text-decoration: none;
    color: block;
    text-align: center;
}
.nav a{
  font-size: 12px;
  color: #666;
}
.nav .active{
    color: #ffbb22;
}
.nav .active .line{
    position: absolute;
    left: 50%;
    bottom: 0;
    background: #ffbb22;
    width:20px;
    height: 2px;
    display: inline-block;
    margin-left: -10px;
}
</style>
