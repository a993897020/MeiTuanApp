<template>
    <transition name="page">
  <div class="contentView" v-if="showFlag" ref="viewContent">
    <div class="wrapper">
        <div class="img-wrapper">
              <img :src="food.picture" alt="" class="foodImg">
              <span class="close-bt icon-close"  @click="closeView"></span>
              <img src="./img/share.png" alt="" class="icon-share">
              <img src="./img/more.png" alt="" class="icon-more">
          </div>
          <div class="desc-wrapper">
              <div class="name">
                {{food.name}}
              </div>
              <div class="totalBuy">
                {{food.month_saled_content}}
              </div>
              <div class="price">
                {{food.min_price}}/{{food.unit}}
              </div>
              <div class="count">
                <app-count-control :food="food" class="foodCount" ></app-count-control>
              </div>
              <div class="buy" @click="selectCount" v-show="!this.food.count||this.food.count==0">
                选规格
              </div>
          </div>
          <div class="assess-wrapper">
            <div class="assess-title">
              {{food.rating.title}}
            </div>
            <div class="good-assess">
              ({{food.rating.like_ratio_desc}}<span>{{food.rating.like_ratio}})</span>
            </div>
            <div class="total-assess">
                    {{food.rating.snd_title}}<span class="icon icon-keyboard_arrow_right"></span>
              </div>
            <div class="assess-content" >
              <ul>
                <li class="assess-item" v-for="(assess,index) in food.rating.comment_list" :key="index">
                  <div class="user-img">
                    <img :src="assess.user_icon" alt="" v-if="assess.user_icon">
                    <img src="./img/anonymity.png" alt="" v-if="!assess.user_icon">
                  </div>
                  <div class="user-name">
                    {{assess.user_name}}
                  </div>
                  <div class="assess-time">
                    {{assess.comment_time}}
                  </div>
                  <div class="content">
                    {{assess.comment_content}}
                  </div>
                </li>
            </ul>
            </div>
          </div>
    </div>
  </div>
    </transition>
</template>

<script>
import CountControl from '../countControl/CountControl'
import Vue from 'vue'
import BScroll from 'better-scroll'
export default {
  components:{
    'app-count-control':CountControl,
  },
    props:{
        food:{
            type:Object,
            default:{}
        }
    },
    created(){
     
    },
  data () {
    return {
      showFlag:false,
      assessScroll:{}
    }
  },
  methods:{
    showView(){
      this.showFlag=true
       this.$nextTick(()=>{
         this.assessScroll=new BScroll(this.$refs.viewContent,{click:true})
      })
      // console.log(this.showFlag)
    },
    closeView(){
      this.showFlag=false
    },
    selectCount(){
      if(!this.food.count||this.food.count!=0){
        Vue.set(this.food,'count',1)
      }
    }
  },
  computed:{
    count(){
      if(this.showFlag==true){
        return '选规格'
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import url(../../common/css/icon.css);
.contentView{
    width: 100%;
    height: 100%;
    position: fixed;
    left: 0;
    bottom: 50px;
    background: #fff;
}
.contentView .img-wrapper{
  position: relative;
  top: 52px;
  left: 0;
  width: 100%;
  height:0;
  padding-top: 100%;
}
.contentView .img-wrapper .foodImg{
  width: 100%;
  height: 100%;
  position: absolute;
  left: 0;
  top: 0;
}
.contentView .img-wrapper .icon-close{
  width: 30px;
  height: 30px;
  background:#666;
  color: #fff;
  border-radius: 50%;
  text-align: center;
  line-height: 30px;
  display: block;
  position: absolute;
  left: 20px;
  top:10px;
  font-size: 22px;
}
.contentView .img-wrapper .icon-share{
  width: 30px;
  height: 30px;
  display:block;
  position:absolute;
  top: 10px;
  right:50px;
}
.contentView .img-wrapper .icon-more{
  width: 30px;
  height: 30px;
  display:block;
  position:absolute;
  top: 10px;
  right: 10px;
}
.contentView .desc-wrapper{
  position: relative;
  top:0;
  left: 0;
  border-bottom:2px solid rgba(0, 0, 0, .1)
}
.contentView .desc-wrapper .name{
  font-size: 16px;
  font-weight: bold;
  padding: 10px;
}
.contentView .desc-wrapper .totalBuy{
font-size: 12px;
padding:10px;
padding-top:0;
color: #999; 
}
.contentView .desc-wrapper .price{
  padding: 10px;
  padding-top: 0;
  font-size: 12px;
  color: red;
}
.contentView .desc-wrapper .count{
 
}
.contentView .desc-wrapper .count .foodCount{
  bottom:10px;
  right: 20px;
}
.contentView .desc-wrapper .buy{
  position:absolute;
  bottom: 10px;
  right: 20px;
  background: #ffd161;
  display: block;
  border-radius:10px;
  padding: 10px;
  font-size: 12px;
}
.contentView .assess-wrapper{
  padding: 10px;
  font-size: 13px;
}
.contentView .assess-wrapper .assess-title{
  display:inline-block;
}
.contentView .assess-wrapper .good-assess{
  display: inline-block;
}
.contentView .assess-wrapper .good-assess span{
  color: red;
  padding: 0 5px;
}
.contentView .assess-wrapper .total-assess{
  float:right;
  color: #999;
}
.contentView .assess-wrapper .total-assess span{
  padding: 0 10px;
}
.contentView  .assess-wrapper .assess-content .assess-item{
border-bottom:1px solid rgba(0, 0, 0, .2);
padding: 10px;
}
.contentView .assess-wrapper .assess-content .assess-item .user-img{
  width: 45px;
  height: 45px;
  float: left;
}
.contentView .assess-wrapper .assess-content .assess-item .user-img img{
  width: 100%;
  height: 100%;
  border-radius: 50%;
}
.contentView .assess-wrapper .assess-content .assess-item .user-name{
  padding: 5px;
  margin-left: 5px;
  font-size: 12px;
  color:#000;
  display: inline-block;
}
.contentView .wrapper .assess-wrapper .assess-content .assess-item .assess-time{
  float: right;
  font-size: 12px;
  color: #666;
  padding: 5px;
}
.contentView .assess-wrapper .assess-content .assess-item .content{
  width: 80%;
  display:inline-block;
  margin-left: 10px;
  font-size: 12px;
  letter-spacing: 1px;
}
.page-enter-active,
.page-leave-active{
  transition: 2s;
}
.page-enter,
.page-leave-to{
  transform: translateX(100%);
  transition-timing-function: ease;
}
.page-leave,
.page-enter-to{
  opacity: 1;
}
</style>
