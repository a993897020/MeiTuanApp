<template>
  <div class="ratings" ref="assess">
    <!-- 评分栏 -->
      <div class="score-wrapper">
        <div class="food-score">
          <div class="score-num">
            {{rating.comment_score}}
          </div>
          <div class="score-text">
            商家评分
          </div>
        </div>
        <div class="star-score">
          <div class="flavor-score">
                <span>口味</span>
            <app-star :score='rating.food_score'></app-star>
          </div>
           <div class="packing-score">
                <span>包装</span>
              <app-star :score='rating.pack_score'></app-star>
           </div>
        </div>
        
        <div class="deliver-score">
          <div class="score-num">
            {{rating.delivery_score}}
          </div>
          <div class="score-text">
            配送评分
          </div>
        </div>
      </div>

      <!-- 导航栏 -->
      <div class="btn-wrapper">
        <div class="btn" v-if="rating.tab">
          <div class="item-btn" @click="selectFn(2)" :class="{'active':selectClick==2}">
             {{rating.tab[0].comment_score_title}}
        </div>
         <div class="item-btn" @click="selectFn(1)" :class="{'active':selectClick==1}">
             {{rating.tab[1].comment_score_title}}
        </div>
         <div class="item-btn" @click="selectFn(0)" :class="{'active':selectClick==0}">
            <img src="./img/icon_sub_tab_dp_highlighted@2x.png" v-if="selectClick==0">
            <img src="./img/icon_sub_tab_dp_normal@2x.png" alt="" v-if="selectClick==2||selectClick==1">
             {{rating.tab[2].comment_score_title}}
        </div>
      </div>
      <div class="label-wrapper" v-for="(label,index) in rating.labels" :key="index">
        <span class="label-item">{{label.content}}{{label.label_count}}</span>
      </div>
      </div>
      <!-- 评价栏 -->
      <div class="content-wrapper" ref="assessScroll">
        <ul class="content-list" >
          <li class="content-item"  v-for="(assess,index) in selectComment" :key="index" >
            <div class="user-img">
              <img :src="assess.user_pic_url" alt="" v-if="assess.user_pic_url">
              <img src="./img/anonymity.png" alt="" v-if="!assess.user_pic_url">
            </div>
            <div class="comment">
            <div class="user-name">
              {{assess.user_name}}
            </div>
            <div class="score">
              <span>评分</span>
              <app-star :score="assess.order_comment_score" class="star"></app-star>
            </div>
            <div class="time">
              {{timers(assess.order_time)}}
            </div>
            <div class="text">
              {{assess.comment}}
            </div>
            <div class="hasImg" v-if="assess.comment_pics[0]">
              <img :src="assess.comment_pics[0].thumbnail_url" alt="">
            </div>
            </div>
          </li>
        </ul>
      </div>
      
  </div>
</template>

<script>
import axios from 'axios'
import Star from '../star/Star'
import BScroll from 'better-scroll'
const ALL=2
const PICTURE=1
const COMMENT=0
export default {
  created(){
    axios.get('./api/ratings')
      .then(res=>{
        // console.log(res.data)
        if(res.data.code==0){
          this.rating=res.data.data
          console.log(this.rating)
        }
        this.$nextTick(()=>{
          this.scroll=new BScroll(this.$refs.assessScroll)
          // console.log(this.scroll)
        })
      })
  },
  components:{
    'app-star':Star
  },
  data () {
    return {
      rating:{},
      selectClick:ALL
    }
  },
  methods:{
    selectFn(index){
     this.selectClick=index
    },
    timers(time){
      // console.log(data)
      let date=new Date(time*1000)
      let year=date.getFullYear();
      let month=date.getMonth()+1;
      let day=date.getDate();
      let _time=year+"-"+month+"-"+day
      // console.log(_time)
      return year+"-"+(month<10?'0'+month:month)+'-'+(day<10?'0'+day:day)
    }
  },
  computed:{
    selectComment(){
      if(this.selectClick==ALL){
        console.log(this.rating.comments)
        return this.rating.comments
      }else if(this.selectClick==PICTURE){
        let list=[]
        this.rating.comments.forEach(item => {
          if(item.comment_pics.length>0){
            list.push(item)
            // console.log(list)
          }
        })
          return list
      }else{
        // console.log(this.rating.comments_dp.comments)
        return this.rating.comments_dp.comments
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.ratings{
  width: 100%;
  
}
.ratings .score-wrapper{
  width: 100%;
  color: #666;
  border-bottom: 10px solid rgba(0, 0, 0, .1);
  display:inline-block;

}
.ratings .score-wrapper .food-score{
  padding: 5px 15px;
  float: left;
}
.ratings .score-wrapper .food-score .score-num{
  font-size: 22px;
  color: #ffb000;
  padding: 5px 10px;
  font-weight: bold;
}
.ratings .score-wrapper .food-score .score-text{
  padding: 5px;
  font-size: 12px;
}
.ratings .score-wrapper .star-score{
  width: 150px;
  float: left;
  font-size: 12px;
  margin-top: 8px;
  display:  block;
}
.ratings .score-wrapper .flavor-score{
  padding: 0 10px;
  display:inline-block;
}
.ratings .score-wrapper .flavor-score span{
  display:inline-block;
  padding: 5px;
}
.ratings .score-wrapper .flavor-score .star{
  display:inline-block;
}
.ratings .score-wrapper .packing-score{
  display:inline-block;
  padding: 5px 10px;
}
.ratings .score-wrapper .packing-score span{
  display:inline-block;
  padding: 5px;
}
.ratings .score-wrapper .packing-score .star{
  display:inline-block;
}

.ratings .score-wrapper .deliver-score{
  float: right;
  padding: 5px;
  color: #666;
  padding-right: 25px; 
}
.ratings .score-wrapper .deliver-score .score-num{
  font-size: 18px;
  padding: 5px;
    margin-left: 10px; 
}
.ratings .score-wrapper .deliver-score .score-text{
  font-size: 12px;
  padding: 5px;
}
.ratings .btn-wrapper{
  padding: 15px;  
}
.ratings .btn-wrapper .btn{
  width: 100%;
  display: inline-block;
}
.ratings .btn-wrapper .btn .item-btn{
  box-sizing: border-box;
  width: 33.3%;
  text-align: center;
  border:1px solid #ffb000;
  padding: 10px;
  font-size: 12px;  
  color: #ffb000;
  float: left;
  border-right:0; 
  height: 33px;
}
.ratings .btn-wrapper .btn .item-btn img{
   width: 15px;
  height: 15px;
  vertical-align: middle;
}
.ratings .btn-wrapper .btn .item-btn:last-child{
  border-right: 1px solid #ffb000;
}
.ratings .btn-wrapper .btn .item-btn.active{
  color: #000;
  background:#ffb000;
}
.ratings .btn-wrapper .label-wrapper{
  float: left;
}
.ratings .btn-wrapper .label-wrapper .label-item{
  padding: 5px 10px;
  background: rgba(0, 0, 0, .1);
  display: inline-block;
  margin-left: 0;
  margin: 5px;
  border-radius: 5px;
  color: rgba(0, 0, 0, .5);
  font-size: 12px;
}
.ratings .content-wrapper{
  width: 100%;
  display:inline-block;
  height: 60%;
  overflow: hidden;
  position: fixed;
  left: 0;
  top: 400px;  
}
.ratings .content-wrapper .content-list{
  display:inline-block;
  padding: 10px;
 position: relative;
  left: 0;
  top:0;
}
.ratings .content-wrapper .content-list .content-item{
  width: 100%;
  display: flex;
  border-bottom: 1px solid rgba(0, 0, 0, .1);
  padding-bottom: 10px;
}
.ratings .content-wrapper .content-list .content-item .user-img{
    flex:0 0 35px;  

}
.ratings .content-wrapper .content-list .content-item .comment{
  flex:1;
  margin-top: 20px;


}
.ratings .content-wrapper .content-list .content-item .user-img img{
  width: 30px;
  height: 30px;
  border-radius: 50%;
}
.ratings .content-wrapper .content-list .content-item .user-name{
  font-size: 11px;
  float: left;
  width: 50%;
  margin-top: -15px;
}
.ratings .content-wrapper .content-list .content-item .score{
  float: left;
  display:block;
  width: 100%;
  margin-right: 50px;
  margin: 10px 0;
}
.ratings .content-wrapper .content-list .content-item .score span{
  font-size: 12px;
  color: #666;
}
.ratings .content-wrapper .content-list .content-item .score .star{
  display:inline-block;
}
.ratings .content-wrapper .content-list .content-item  .time{
  float:right;
  margin-top: -50px;
  font-size: 12px;
  color: #666;
}
.ratings .content-wrapper .content-list .content-item .comment .text{
  display: block;
  float: left;
  width: 100%;
}
.ratings .content-wrapper .content-list .content-item .comment .hasImg{
  width: 150px;
  height: 80px;
  float: left;
  margin-top: 10px;
}
.ratings .content-wrapper .content-list .content-item .comment .hasImg img{
  width: 100%;
  height: 100%;
  border-radius: 5px;
}
</style>
