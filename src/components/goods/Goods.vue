<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuScroll">
      <ul>
        <li class="menuItem" v-if="data.container_operation_source" :class="{'pos':position===0}" @click="menuClick(0)">
          <p class="text">
            <img :src="data.container_operation_source.tag_icon" alt="" >
            {{data.container_operation_source.tag_name}}
          </p>
        </li>
        <li class="menuItem"
         v-for="(food,index) in data.food_spu_tags"
        :key="index" 
         :class="{'pos':position===index+1}" 
        @click="menuClick(index+1)">
          <p class="text">
            <img :src="food.icon" alt="" v-if="food.icon">
            {{food.name}}
          </p>
          <i class="num" v-show='count(food.spus)'>
            {{count(food.spus)}}
          </i>
        </li>
      </ul>
    </div>
    <div class="content-wrapper" ref="foodScroll" >
      <ul class="contentImg" >
        <li class="item-hook">
          <div v-for="(imgItem,index) in img.operation_source_list" :key="index">
            <img :src="imgItem.pic_url" alt="">
          </div>
        </li>
        <li class="item-hook" v-for="(item,index) in data.food_spu_tags" :key="index"> 
      <ul class="classList" >
        <li class="classItem"  >
          <div class="classText" >
            <div class="icon" > </div>
            {{item.name}}
            <div class="fire" :style="fire" v-if="item.icon" > </div>
          </div>
          <ul class="contentList" v-for="(content,index) in item.spus" :key="index">
            <li class="contentItem"  @click.stop.prevent="contentView(content)" >
             <div class="img">
                <img :src="content.picture" alt="">
             </div>
              <div class="text">
                <p class="title">
                  {{content.name}}
                </p>
                <div class="desc" v-if="content.description">
                  {{content.description}}
                </div>
                <p class="buy">
                  {{content.month_saled_content}} <span>{{content.praise_content}}</span>
                </p>
                <div class="good" v-if="content.product_label_picture" >
                  <img :src="content.product_label_picture" alt="">
                </div>
                <div class="money">
                  ${{content.min_price}}/{{content.unit}}
                  </div>
                  <app-count :food='content' ></app-count>
              </div>
              
            </li>
          </ul>
        </li>
      </ul>
    </li>
   </ul>
    </div>
    <div class="shopping-wrapper">
      <app-shopping :data="data" :selectFoods='selectFoods' ></app-shopping>
    </div>
       <app-content-view :food="itemFood" ref="view" ></app-content-view>
  </div>
</template>

<script>
import axios from 'axios'
import BScroll from 'better-scroll'
import Shopping from '../shopping/Shopping'
import Vue from 'vue'
import CountControl from '../countControl/CountControl'
import ContentView from '../contentView/ContentView'
export default {
 components:{
   'app-shopping':Shopping,
   'app-count':CountControl,
   'app-content-view':ContentView
 },
  data () {
    return {
      data:{},
      img:{},
      foodScroll:{},
      menuScroll:{},
      ScrollY:0,
      listHeight:[],
      hasCount:false,
      goods:[],
      view:false,
      itemFood:{}
    }
  },
  created(){
    axios.get('/api/goods')
    .then(res=>{
      if(res.data.code==0){
        this.data=res.data.data;
        this.img=res.data.data.container_operation_source
        this.goods=res.data.data.food_spu_tags
        console.log(this.data)
        this.$nextTick(()=>{
          this.initScroll()
          this.interval()
        })
      }
    })
    axios.get(' https://easy-mock.com/mock/5d3aff29e40db078474ba18f/example/mock')
      .then((res)=>{
        console.log(res.data)
      })
  },
  computed:{
    fire(){
      return 'background-image:url('+ this.data.food_spu_tags[0].icon +')'
    },
    position(){
      for(let i=0;i<this.listHeight.length;i++){
        let height1=this.listHeight[i]
        let height2=this.listHeight[i+1]
        if(!height2||(this.ScrollY>height1&&this.ScrollY<height2)){
          return i
        }
      }
    },
     selectFoods(){
        let foods=[]
        this.goods.forEach((myfoods)=>{
          myfoods.spus.forEach((food)=>{
            if(food.count>0){
              foods.push(food)
            }
          })
        })
        return foods
      }
  },
  methods:{
    initScroll(){
        this.foodScroll = new BScroll(this.$refs.foodScroll,{probeType:3,click:true})//probeType为滚动事件  默认为0不产生事件 3时刻产生事件
        this.menuScroll = new BScroll(this.$refs.menuScroll,{click:true})

        this.foodScroll.on("scroll",(pos)=>{
          this.ScrollY=Math.abs((Math.round(pos.y))) //Math.abs绝对值 Math.round向上取整 Math.flood向下取整
          // console.log(this.ScrollY)
        })
    },
    interval(){ //每个分类的区间获取 用于样式处理
      let foodItem=this.$refs.foodScroll.getElementsByClassName('item-hook');
      // console.log(foodItem)
      let height=0;
      this.listHeight.push(height)
      // console.log(this.listHeight)
      for(let i=0;i<foodItem.length;i++){
        let item=foodItem[i]
        // console.log(item)
        height+=item.clientHeight
        this.listHeight.push(height)
      }
        // console.log(this.listHeight)
    },
    menuClick(index){  //点击菜单，显示到对应区域
      // console.log(index)
      let foodItem=this.$refs.foodScroll.getElementsByClassName('item-hook');
      let ele=foodItem[index]
      // console.log(ele)
      this.foodScroll.scrollToElement(ele,250)
    },
    count(spus){
      // console.log(spus)
      let count = 0;
      spus.forEach(food => {
        if(food.count>0){
        count+=food.count
        // console.log(count)
        }
      });
      return count
    },
    contentView(content){
      this.itemFood=content
      this.$refs.view.showView()
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import url(../../common/css/icon.css);
.goods{
  display: flex;
  width: 100%;
  position:absolute;
  top: 190px;
  bottom: 50px;
}
.goods .menu-wrapper{
  flex:0 0 85px;
  overflow: hidden;
}
.goods .menu-wrapper .menuItem{
  padding:20px 3px 15px 20px;
  border-bottom: 1px solid rgba(0, 0, 0, .1);
  background:rgba(255,255,255,.5);
  position: relative;
}
.goods .menu-wrapper .menuItem .text{
  display: -webkit-box;
  -webkit-line-clamp:2;
  -webkit-box-orient: vertical;
  font-size: 13px;
  overflow: hidden;
}
.goods .menu-wrapper .menuItem .num{
  width: 14px;
  height: 14px;
  position:absolute;
  top: 5px;
  left:70px;
  font-size: 10px;
  background-color: red;
  color: #fff;
  border-radius:50%;
  text-align: center;
  line-height: 14px;
  font-weight: normal;
}
.goods .menu-wrapper .text img{
  width: 15px;
  height: 15px;
}
.goods .content-wrapper{
  flex:1;
  overflow: hidden;
}
.goods .content-wrapper .contentImg{
  padding: 10px;
  border: 1px solid rgba(0, 0, 0, .1);
  
}
.goods .content-wrapper  img{
  width: 100%;
  margin-bottom: 10px;
  border-radius: 5px;
}
.goods .content-wrapper .classItem .classText{
  font-size: 14px;
  margin-bottom: 20px;
}
.goods .content-wrapper .classItem .classText .icon{
   background: url(./img/btn_yellow_highlighted@2x.png) no-repeat;
  background-size: 5px 15px;
  padding-left: 15px;
  background-position:center;
  display: inline;
}
.goods .content-wrapper .classItem .classText .fire{
  width: 15px;
  height: 15px;
  margin-top: 10px;
  background-repeat: no-repeat;
  background-size: 100% 100%;
  background-position:center;
  display: inline-block; 
}
.goods .content-wrapper .contentList{
  position:relative;
}
.goods .content-wrapper .contentList .contentItem{
  display: flex;
  margin-bottom: 20px;
}
.goods .content-wrapper .contentList .contentItem .img{
  min-width: 80px;
  max-width: 80px;
  height: 80px;
}
.goods .content-wrapper .contentList .contentItem .img img{
  width: 100%;
  height: 100%;
}
.goods .content-wrapper .contentList .contentItem .text{
  font-size: 12px;
  color: rgba(0, 0, 0, .5);

}
.goods .content-wrapper .contentList .contentItem .text p{
  padding: 5px;
}
.goods .content-wrapper .contentList .contentItem .text .title{
  font-size: 16px;
  color: #000;
}
.goods .content-wrapper .contentList .contentItem .text .buy span{
  padding:10px;
}
.goods .content-wrapper .contentList .contentItem .text .money{
  color: red;
  padding: 5px;
}
.goods .content-wrapper .contentList .contentItem .text .good{
  border-radius:5px;
  width: 80px;
  height: 15px;
}
.goods .content-wrapper .contentList .contentItem .text .desc{
  padding: 5px;
}
.goods .content-wrapper .contentList .contentItem .text .good img{
  width: 100%;
  height: 100%;
}
.goods .menu-wrapper .pos{
  background: rgba(0,0,0,.1);
  border: 1px solid rgba(0, 0, 0, .1);
  font-weight: bold;
}
.goods .shopping-wrapper{
  position:fixed;
  bottom: 0;
  left: 0;
  background:rgba(0, 0, 0, .7);
  width: 100%;
  height: 50px;
  z-index: 99;
}
</style>
