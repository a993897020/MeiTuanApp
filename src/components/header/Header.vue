<template>
  <div class="header">
      <!-- 上 -->
      <div class="top-wrapper">
          <div class="back-wrapper">
              <span class="icon-arrow_lift"></span>
          </div>
          <form class="search-wrapper">
              <span class="search-icon"></span>
              <input type="text" class="search" placeholder="请输入搜索内容">
          </form>
          <div class="more-wrapper">
              <a href="#" class="pindan">拼单</a>
              <div class="point-wrapper">
              <i class="point"></i>
              <i class="point"></i>
              <i class="point"></i>
              </div>
          </div>
      </div>

      <!-- 中 -->
      <div class="content-wrapper">
          <div class="mImg" :style="pic_url"></div>
          <div class="title">
              {{poiInfo.name}}
          </div>
          <div class="star">
              <img src="./img/star.png" alt="">
              <span>收藏</span>
          </div>
      </div>
      <div class="bgImg" :style="head_pic_url"> 
      </div>

      <!-- 下 -->
      <div class="footer-wrapper">
          <img v-if="poiInfo.discounts2" :src="poiInfo.discounts2[0].icon_url" alt="">  <!-- 图片 首 -->
          <div class="text" v-if="poiInfo.discounts2">
              {{poiInfo.discounts2[0].info}}
          </div>
          <div class="active" v-if="poiInfo.discounts2" @click="open">
              {{poiInfo.discounts2.length}}个活动
          </div>
      </div>
    <transition name='open'>
        <!-- 隐藏内容 -->
        <div class="hidder-wrapper" v-show="show">
            <div class="hidder-content">
            <div class="hidderImg" :style="poi_back_pic_url">
                <div class="mImg" :style="pic_url"></div>
                <div class="title">
                        {{poiInfo.name}}
                 </div>
                 <div class="star">
                     <app-star :score="poiInfo.wm_poi_score"></app-star>
                     {{poiInfo.wm_poi_score}}
                 </div>
                 <div class="text">
                     <span>{{poiInfo.min_price_tip}}</span>|<span>{{poiInfo.shipping_fee_tip}}</span>|<span>{{poiInfo.delivery_time_tip}}</span>
                 </div>
                 <div class="text">
                     <span>配送时间：{{poiInfo.shipping_time}}</span>
                 </div>
                 <hr>
                  <img v-if="poiInfo.discounts2" :src="poiInfo.discounts2[0].icon_url" alt="">  <!-- 图片 首 -->
                  <div class="imgText" v-if="poiInfo.discounts2">
                        {{poiInfo.discounts2[0].info}}
                  </div>
                  <div class="close">
                      <span class="icon-close" @click="hidder"></span>   
                  </div>
            </div>
        </div>
    </div>
    </transition>
  </div>
</template>

<script>
import Star from '../star/Star'
export default {
    components:{
        'app-star':Star
    },
    props:{
        poiInfo:{
            type:Object,
            default:{}
        }
    },
  data () {
    return {
        show:false
    }
  },
  computed:{
      head_pic_url(){
          return 'background-image:url('+ this.poiInfo.head_pic_url +');'//背景图片
      },
      pic_url(){
          return 'background-image:url('+ this.poiInfo.pic_url +');' //图片M
      },
     poi_back_pic_url(){
         return 'background-image:url('+ this.poiInfo.poi_back_pic_url +');' //隐藏的背景图片 
     }
  },
  methods:{
      open(){
          this.show=true
      },
      hidder(){
          this.show=false
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import url(../../common/css/icon.css);
.header{
    width: 100%;
    margin-top: 20px;
    height: 130px;
}
.header .top-wrapper{
    position: relative;
}
.header .top-wrapper .back-wrapper{
   position:absolute;
   top: 3px;
   left: 10px;
    width: 30px;
    height: 20px;
    display: inline-block;
    line-height: 20px;
    cursor:pointer;
}
.header .top-wrapper .back-wrapper span{
    display:inline-block;
    color: #fff;
}
.header .top-wrapper .search-wrapper{
    width: 100%;
    display: inline-block;
    padding: 0 100px 0 50px;
    box-sizing: border-box;
}
.header .top-wrapper .search-wrapper .search{
    width: 100%;
    background: #fff;
    border-radius: 15px;
    border: 0;
    height: 25px;
    outline: none;
    padding-left: 25px;
}
.header .top-wrapper .search-wrapper .search-icon{
    position: absolute;
    left: 55px;
    top:6px;
    background: url(./img/search.png) no-repeat;
    color: #fff;   
   width: 30px;
   height: 30px;
   background-size: 14px 14px;
}
.header .top-wrapper .more-wrapper{
    position:absolute;
    right: 5px;
    bottom: 5px;
}
.header .top-wrapper .more-wrapper .pindan{
    text-decoration: none;
    color: #FFF;
    border: 1px solid #fff;
    margin-right:30px;
    font-size: 12px;
    padding: 2px 3px;
}
.header .top-wrapper .point-wrapper{
    float: right;
    position: absolute;
    right: 5px;
    top: 5px;
    cursor: pointer;
}
.header .top-wrapper .more-wrapper .point-wrapper .point{
    width: 3px;
    height: 3px;
    border: 1px solid #fff;
    border-radius:50%;
    display:block;
    float: left;
    margin-right: 1px;        
}
.header .content-wrapper{
    height: 50px;
    padding: 18px 10px 5px 10px;
}
.header .content-wrapper .mImg{
    width: 50px;
    height: 50px;
    background-size: 100% 100%;
    background-position: center;
    border-radius: 5px;
    float: left;
}
.header .content-wrapper .title{
    color: #FFF;
    float: left;
    margin: 15px 10px;
    font-weight: bold;
}
.header .content-wrapper .star{
    float:right;
    margin-top: 10px;
}
.header .content-wrapper .star img{
    width: 20px;
    height:20px;
}
.header .content-wrapper .star span{
    color: #FFF;
    font-size: 12px;
    display: block;
}
.header .footer-wrapper{
    padding: 5px 10px;
}
.header .footer-wrapper img{
    width: 15px;
    height: 15px;
    float: left;  
    margin-right: 10px;

}
.header .footer-wrapper .text{
    font-size: 12px;
    color: #fff;
    display: inline;
}
.header .footer-wrapper .active{
    float:right;
    color: #fff;
    font-size: 12px;
    cursor: pointer;

}
.header .hidder-wrapper{
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, .6);
    z-index: 999;
    position: absolute;
    top: 0;
    left: 0;
}
.header .hidder-wrapper .hidder-content{
    width: 100%;
    height: 100%;
    padding: 60px 40px 100px 20px;
    box-sizing: border-box;
}
.header .hidder-wrapper .hidder-content .hidderImg{
    width: 100%;
    height: 90%;
    border-radius: 10px;
    text-align: center;
    padding:20px 10px 5px 10px;
    background-repeat: no-repeat;
    background-size: 100% 100%;
}
.header .hidder-wrapper .hidder-content .hidderImg .mImg{
    width: 60px;
    height: 60px;
    background-size: 130% 100%;
    background-position:center;
    display: inline-block;
    margin-bottom: 10px;
    border-radius: 10px;
}
.header .hidder-wrapper .hidder-content .hidderImg .title{
    color: #eee;
    font-size: 16px;
    margin-bottom: 10px;
}
.header .hidder-wrapper .hidder-content .hidderImg .text{
    font-size: 11px;
    color: #bababc;
    margin-bottom: 10px;

}
.header .hidder-wrapper .hidder-content .hidderImg span{
    padding: 10px;
}
.header .hidder-wrapper .hidder-content .hidderImg .hr{
    color: #bababc;
    height: 1px;
}

.header .hidder-wrapper .hidder-content .hidderImg img{
    width: 15px;
    height: 15px;
    vertical-align: middle;
}
.header .hidder-wrapper .hidder-content .hidderImg .imgText{
    color: #eee;
    font-size: 12px;
    display: inline-block;
    margin-top: 20px;
}
.header .hidder-wrapper .hidder-content .hidderImg .close{
    width: 40px;
    height: 40px;
    position: fixed;
    bottom: 50px;
    left: 50%;
    border-radius: 50%;
    line-height: 40px;
    background: rgba(0, 0, 0, .2);
    display: inline-block;
    color: rgba(255, 255, 255, .7);
    margin-left: -20px;
    cursor: pointer;
}
.header .hidder-wrapper .hidder-content .hidderImg .star{
    display:inline-block;
    margin-bottom: 10px;
    font-size: 12px;
    color: #fff;
    padding-right: 5px;
}
.open-enter-active,
.open-leave-active{
    transition: 2s;
}
.open-enter,
.open-leave-to{
    opacity: 0;
}
.open-leave,
.open-enter-to{
    opacity: 1;
}
.header .bgImg{
    width: 100%;
    height: 150px;
    z-index:-1;
    position:absolute;
    top: 0;
    left: 0;
    background-size: 100% 135%;
    background-position: center -10px;
}

</style>
