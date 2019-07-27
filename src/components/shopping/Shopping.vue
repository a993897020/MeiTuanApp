<template>
  <div class="shopping" > 
      <div class="logo-wrapper" @click="shoppingView">
      <span class="icon-shopping_cart logo" :class="{'active':totalCount>0}"></span>
      <div v-for="(item,index) in data.food_spu_tags" :key="index">
         <i class="count" v-show="totalCount" :class="{'active':totalCount>0}">{{totalCount}}</i>
      </div>
      </div>

      <div class="view-wrapper" v-if="button"   >
          <div class="bg"  @click="hidder"></div>
          <div class="shopping-view">
              <div class="header" v-if="data.poi_info.discounts2">
              {{data.poi_info.discounts2[0].info}}
          </div>
            <div class="title">
                <div class="title-left">
                    1号口袋
                </div>
                <div class="title-right" @click="clearFood">
                    <img src="./img/ash_bin.png" alt="">
                    <span>清空购物车</span>
                </div>
            </div>
            <div class="content"  ref="list">
                <ul>
                    <li v-for="(food,index) in selectFoods" :key="index" class="content-item">
                        <div class="desc">
                            <div class="name">
                                {{food.name}}
                            </div>
                            <div class="price">
                               ${{food.min_price}}
                            </div>   
                        </div>
                        <div class="count">
                            <app-count-control :food='food' class="count-control" ></app-count-control>
                        </div>
                        <div class="unit">
                            {{food.unit}}
                        </div>
                    </li>
                </ul>
            </div>
          </div>
          
            
      </div>


      <div class="desc-wrapper" v-if="data.poi_info">
          <div class="left-price">
              <div class="totalPrice" :class="{'active':totalCount>0}" v-if="totalCount">
                  ${{totalPrice}}
              </div>
              <div class="goPrice" :class="{'active':totalCount>0}" >
              另需{{data.poi_info.shipping_fee_tip}}
              </div>
          </div>
          <div class="right-price" :class="{'active':totalCount>0}">
              {{rightData}}
          </div>
      </div>
  </div>
</template>

<script>
import CountControl from '../countControl/CountControl'
import BScroll from 'better-scroll'
export default {
    components:{
        'app-count-control':CountControl,
        },
    props:{
        data:
        {
        type:Object,
        default:{}
        },
        selectFoods:{
            type:Array,
            default(){
                return [
                ]
            }
        }
    },
  
  data () {
    return {
            button:false,
            shopScorll:{}
    }
  },
  
  methods:{
    shoppingView(){
        if(!this.totalCount){
            this.button==false;
        }else{
        this.button=!this.button;
         this.$nextTick(()=>{
                this.shopScorll=new BScroll(this.$refs.list)
            })
        }
    },
    clearFood(){
        this.selectFoods.forEach(food=>{
            food.count=0
            this.button=false;
        })
    },
    hidder(){
        if(this.button==true){
            this.button=false;
        }
    }
  },
  computed:{
    totalCount(){
    let num=0
    this.selectFoods.forEach(food => {
        num+=food.count
    });
        return num
},
    totalPrice(){
        let total=0
        this.selectFoods.forEach(food=>{
            total+=food.min_price*food.count
        })
         return total
    },
    rightData(){
        if(this.totalCount>0){
            return "去结算"
        }else{
            return this.data.poi_info.min_price_tip
        }
    },
   
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import url(../../common/css/icon.css);
.shopping{
    position: relative;
    z-index: 99
}
.shopping .logo-wrapper{
    width:50px;
    height: 50px;
    background: #666;
    border-radius:50%;
    position:absolute;
    left: 20px;
    top: -15px;
    z-index: 99
}
.shopping .logo-wrapper .logo{
    text-align: center;
    line-height: 50px;
    font-size: 30px;
    color: rgba(255,255,255,.5);
    display:inline-block;
    margin-left: 10px;
}

.shopping .desc-wrapper .left-price{
    display:inline-block;
    margin-left: 75px;
}
.shopping .desc-wrapper .left-price .totalPrice.active{
    font-size: 18px;
    margin: 8px 0;
    color: #fff;
}
.shopping .desc-wrapper .left-price .goPrice{
    font-size: 12px;
    color: rgba(255, 255, 255, .5);
    margin-top: 15px;
}
.shopping .desc-wrapper .left-price .goPrice.active{
    font-size: 12px;
    color: rgba(255,255,255,.5);
    margin: 0;
}
.shopping .desc-wrapper .right-price{
    font-size: 16px;
    font-weight: bold;
    color: rgba(255, 255, 255, .5);
    float:right;
    line-height: 50px;
    width: 100px;
    text-align: center;
}
.shopping .logo-wrapper .count.active{
    width: 14px;
    height: 14px;
    color: #fff;
    font-size: 12px;
    position:absolute;
    top:-1px;
    left: 35px;
    background-color: red;
    border-radius: 50%;
    text-align: center;
    line-height: 14px; 
}
.shopping .logo-wrapper .logo.active{
    background: #ffd161;
    border-radius: 50%;
    width: 50px;
    height: 50px;
    color: #000;
    position:absolute;
    top: 0;
    left: -11px;
}
.shopping .desc-wrapper .right-price.active{
    background:#ffd161;
    color: #000;
    display:inline-block;
    width: 100px;
    text-align: center;
}
.shopping .view-wrapper{
    position: fixed;
    width: 100%;
    height: 79%;
    top: 0;
    left: 0;
}
.shopping .view-wrapper .bg{
    width: 100%;
    height: 100%;
    background:rgba(0, 0, 0, .3);
}
.shopping .view-wrapper .header{
    width: 100%;
    background: #ffd161;
    color: #000;
    text-align: center;
    font-size: 12px;
    padding:10px 0;
}

.shopping .view-wrapper .shopping-view{
    position:fixed;
    bottom: 50px;
    width: 100%;
}
.shopping .view-wrapper .shopping-view .title{
    background-color: #fff;
    border-left: 5px solid green;
    border-top:1px solid rgba(0, 0, 0, .1);
    border-bottom:1px solid rgba(0, 0, 0, .1);
    font-size: 12px;
    padding: 10px;
}
.shopping .view-wrapper .shopping-view .title .title-right{
    margin-top: -15px;
    float:right;
}
.shopping .view-wrapper .shopping-view .title .title-right img{
    width: 15px;
    height: 15px;
    vertical-align:middle;
}
.shopping .view-wrapper .shopping-view .title .title-right span{
    display:inline-block;
    font-size: 12px;
    vertical-align: -1px;
}
.shopping .view-wrapper .shopping-view .content{
    width: 100%;
    background-color: #fff;
    max-height: 300px;
    overflow: hidden;
}
.shopping .view-wrapper .shopping-view .content .content-item{
    position: relative;
    width: 100%;
    left: 0;
    top: 0;
    border-bottom: 1px solid rgba(0, 0, 0, .1);
    padding:5px 0;
}
.shopping .view-wrapper .shopping-view .content .desc{
    width: 280px;
    padding: 10px;
    float: left;
}
.shopping .view-wrapper .shopping-view .content .name{
    float: left;
}
.shopping .view-wrapper .shopping-view .content .price{
    float:right;
    margin-right: 10px;
}
.shopping .view-wrapper .shopping-view .count{
    display:inline-block;
}
.shopping .view-wrapper .shopping-view .count .count-control{
    bottom: 25px;
    right: 5px;
}
.shopping .view-wrapper .shopping-view .unit{
    font-size: 12px;
    color:#666;
    padding: 0px 10px;
    display: block;
    clear: both;
    display:block;

}

</style>
