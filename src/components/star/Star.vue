<template>
  <div class="star">
   <span class="item" :class="item" v-for="(item,index) in list" :key="index" ></span>
  </div>
</template>

<script>
const LENGTH=5;
const ITEM_ON='on'      //满星
const ITEM_HALF='half' //半星
const ITEM_OFF='off'  //空星
export default {
    props:{
        score:{
            type:Number
        }
    },
  data () {
    return {
    }
  },
  computed:{
      list(){
        let result=[]
        //分数:4.7
        let starScore=Math.floor(this.score*2)/2 //Math.floor(4.7*2)=9
        let starHalf=starScore%1!==0 //4.5%1=5 
        let starFull=Math.floor(starScore) //满星数量
        for(let i=0;i<starFull;i++){  //满星遍历
            result.push(ITEM_ON)
        }
        if(starHalf){ //是否有半星
            result.push(ITEM_HALF)
        }
        if(result.length<LENGTH){
            result.push(ITEM_OFF)
        }
        // console.log(result)
        return result
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.star{
    font-size: 0;
}
.item{
    width: 10px;
    height: 10px;
    display:inline-block;
    background-size: 100% 100%;
    margin-right: 5px;
}
.item:last-child{
    margin-right: 0;
}
.star .on{
    background-image: url(img/star24_on@2x.png);
    
}
.star .half{
    background-image: url(./img/star24_half@2x.png);
}
.star .off{
    background-image: url(./img/star24_off@2x.png);
}
</style>
