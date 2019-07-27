<template>
  <div class="countControl">
  <transition name='move'>

                    <div class="removeCount icon-remove_circle_outline" @click.stop.prevent="removeCount" v-if="this.food.count>0" ></div>
    </transition>

                    <div class="count" v-if="this.food.count>0">{{food.count}}</div>
                    <div class="addCount icon-add_circle" @click.stop.prevent="addCount">
                      <i class="bg"></i>
                    </div>

  </div>
</template>

<script>
import Vue from 'vue'
export default {
    props:{
        food:{
            type:Object,
            default:{}
        }
    },
  data () {
    return {
    }
  },
  methods:{
      removeCount(){
          this.food.count--
      },
      addCount(){
          if(!this.food.count){
              Vue.set(this.food,"count",1)
          }else{
              this.food.count++
          }
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import url('../../common/css/icon.css');
.countControl{
 position:absolute;
 right: 5px;
 bottom: 20px;
}
.countControl .removeCount{
  font-size: 22px;
 display:inline-block;
 vertical-align: middle;
}
.countControl .count{
  
  padding: 0 5px;
  font-size: 14px;
 display:inline-block;
 text-align: center;
 color: #666;
 vertical-align:middle;
 text-align: center;
}
.countControl .addCount{
  font-size: 25px;
  width: 25px;
  height: 25px;
  display:inline-block;
  color: #ffd161;
 vertical-align:middle;
  position: relative;
  bottom: 0;
  right: 0px;
}
.countControl .addCount .bg{
  position:absolute;
  bottom: 5px;
  right:  3px;
  width: 19px;
  height: 18px;
  border-radius: 50%;
  display:inline-block;
  background: #000;
  z-index: -1;
}
.move-enter-active,
.move-leave-active{
  /* 动画的时长 */
    transition: 2s;
}
.move-enter,
.move-leave-to{      
  /* move-enter同时执行动画和事件 move-leave-to先执行事件后动画 */
    /* transition-timing-function: linear; */
    /* opacity: 0; */
    transition-timing-function: linear; 
    transform: translate(28px) rotate(180deg);
}
.move-leave,
.move-enter-to{
    /* opacity: 0; */
}

</style>
