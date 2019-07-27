<template>
  <div class="seller">
    <div class="content-wrapper">
      <div class="address">
        <div class="text">
          <img src="./img/address.png" alt="">
          <span>{{seller.address}}</span>
        </div>
        <div class="phone"></div>
      </div>
      <div class="picture" ref="picture">
        <ul v-if="seller.poi_env" class="picture-list" ref="imgList">
          <li v-for="(item,index) in seller.poi_env.thumbnails_url_list" :key="index"  class="picture-item" ref="item">
              <img :src="item" alt="">
          </li>
        </ul>
      </div>
      <div class="desc">
        <img src="./img/safety.png" alt="">
        <span>查看食品安全档案</span>
      </div>
      <div class="time">
        <p>配送服务:{{seller.app_delivery_tip}}</p>
        <div class="desc">
          <img src="./img/time.png" alt="">
          <span>配送时间:{{seller.shipping_time}}</span>
        </div>
      </div>
      <div class="desc" v-if="seller.poi_service">
        <img src="./img/server.png" alt="">
        <span>商家服务</span>
        <img :src="seller.poi_service[0].icon" alt="">
        <span>{{seller.poi_service[0].content}}</span>
      </div>
      <div class="desc" v-if="seller.discounts2">
        <img :src="seller.discounts2[0].icon_url" alt="">
        <span>{{seller.discounts2[0].info}}</span>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import BScroll from 'better-scroll'
export default {
  data () {
    return {
      seller:{},
    }
  },
  created(){
    axios.get("api/seller")
      .then((res)=>{
        // console.log(res.data)
        if(res.data.code==0){
          this.seller=res.data.data
          console.log(this.seller)
        }
        this.$nextTick(()=>{
          let liW=0
          liW=this.$refs.item[0].clientWidth;
          let width=liW*this.$refs.item.length;
          this.$refs.imgList.style.width=width+'px';
          console.log(width)
          this.scroll=new BScroll(this.$refs.picture,{
          scrollX:true,
          click:true
          })
          console.log(this.scroll)
        })
      })
     
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.seller{
  position: relative;
}
.seller .content-wrapper{
  padding: 20px;
}
.seller .content-wrapper .address{
  width: 100%;
}
.seller .content-wrapper .address .text{
  font-size: 14px;
  display:inline-block;
}
.seller .content-wrapper .address .text img{
  width: 15px;
  height: 15px;
  margin-right:10px;
  vertical-align:top;
}
.seller .content-wrapper .address .phone{
  float:right;
  width: 20px;
  height: 20px;
  background-image: url('./img/phone.png');
  background-size: 100% 100%;
  background-repeat:no-repeat;
}
.seller .content-wrapper .picture{
  padding: 20px 0;
  padding-bottom: 0;
  width: 100%;
  overflow: hidden;
  position: relative;
  height: 80px;
  }
.seller .content-wrapper .picture .picture-list{
  position: absolute;
}
.seller .content-wrapper .picture .picture-list .picture-item{
  width: 80px;
  height: 80px;
  padding-right: 8px;
  display: inline-block;
}
.seller .content-wrapper .picture .picture-list .picture-item img{
  width: 100%;
  height: 100%;
  border-radius: 5px;
}
.seller .content-wrapper .desc{
  width: 100%;
  padding: 20px 0;
  display:inline-block;
  font-size: 14px;
}
.seller .content-wrapper .desc img{
  width: 15px;
  height: 15px;
  vertical-align: top;
}
.seller .content-wrapper .desc span{
  margin-left: 10px;
  margin-right: 5px;
}
.seller .content-wrapper .time p{
  font-size: 14px;
}
</style>
