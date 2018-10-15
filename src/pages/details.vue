<template>
    <div class="detail">
      <div v-if="$route.params.id == value.id" class="detail_list" v-for="(value,index) in list" :key="index">
        <div class="detail_img">
          <img :src="value.images" alt="">
          <span @click="go_back" class="detail_back"><i class="fa fa-chevron-left"></i></span>
        </div>
        <div class="content_text">
          <span style="display: inline-block;padding-top: 10px">{{value.title}}</span>
          <div style="padding: 10px 0">
            <span>{{value.detail}}</span>
          </div>
          <div>
            <span>价格：</span>
            <span style="color: orangered">{{value.price}}.00</span>
          </div>
        </div>
      </div>
      <mt-button class="primary" type="primary" @click="toCar">加入购物车</mt-button>
    </div>
</template>

<script>
  // import {mapActions} from 'vuex'
    export default {
        name: "detail",
      data(){
          return{
            list:[
              {
                id:'1',
                title:'陶瓷杯',
                price:100,
                images:'http://cdn.qiqiangkeji.com/20180926.fff7f6be283261b884d7f400be65e1b1_1024x710.jpg',
                detail:'手工制作，值得拥有'
              },
              {
                id:'2',
                title:'玻璃杯',
                price:200,
                images:'http://cdn.qiqiangkeji.com/20180926.9383d209c5395f8598ca9a2731696a94_850x680.jpg',
                detail:'手工制作，值得拥有，手工制作，值得拥有，快点加入购物车吧！'
              },
              {
                id:'3',
                title:'高级吸汗毛巾',
                price:10,
                images:'http://cdn.qiqiangkeji.com/20180926.83a59e1a91b31003fc2b2beffd7702b7_320x220.jpg',
                detail:'手工制作，值得拥有'
              },
              {
                id:'4',
                title:'纯棉围巾',
                price:120,
                images:'http://cdn.qiqiangkeji.com/20180926.3015a6dd5205bb599914c7b972f11f04_500x500.jpg',
                detail:'手工制作，值得拥有，手工制作，值得拥有，快点加入购物车吧！'
              },
              {
                id:'5',
                title:'陶瓷杯1',
                price:100,
                images:'http://cdn.qiqiangkeji.com/20180926.fff7f6be283261b884d7f400be65e1b1_1024x710.jpg',
                detail:'手工制作，值得拥有，快点加入购物车吧！'
              },
              {
                id:'6',
                title:'玻璃杯1',
                price:200,
                images:'http://cdn.qiqiangkeji.com/20180926.9383d209c5395f8598ca9a2731696a94_850x680.jpg',
                detail:'手工制作，值得拥有'
              },
              {
                id:'7',
                title:'高级吸汗毛巾1',
                price:10,
                images:'http://cdn.qiqiangkeji.com/20180926.83a59e1a91b31003fc2b2beffd7702b7_320x220.jpg',
                detail:'手工制作，值得拥有，手工制作，值得拥有，快点加入购物车吧！'
              },
              {
                id:'8',
                title:'纯棉围巾1',
                price:120,
                images:'http://cdn.qiqiangkeji.com/20180926.3015a6dd5205bb599914c7b972f11f04_500x500.jpg',
                detail:'手工制作，值得拥有，手工制作，值得拥有，快点加入购物车吧！'
              },
              {
                id:'9',
                title:'陶瓷杯2',
                price:100,
                images:'http://cdn.qiqiangkeji.com/20180926.fff7f6be283261b884d7f400be65e1b1_1024x710.jpg',
                detail:'手工制作，值得拥有，快点加入购物车吧！'
              },
              {
                id:'10',
                title:'玻璃杯2',
                price:200,
                images:'http://cdn.qiqiangkeji.com/20180926.9383d209c5395f8598ca9a2731696a94_850x680.jpg',
                detail:'手工制作，值得拥有'
              },
              {
                id:'11',
                title:'高级吸汗毛巾2',
                price:10,
                images:'http://cdn.qiqiangkeji.com/20180926.83a59e1a91b31003fc2b2beffd7702b7_320x220.jpg',
                detail:'手工制作，值得拥有，手工制作，值得拥有，快点加入购物车吧！'
              },
              {
                id:'12',
                title:'纯棉围巾2',
                price:120,
                images:'http://cdn.qiqiangkeji.com/20180926.3015a6dd5205bb599914c7b972f11f04_500x500.jpg',
                detail:'手工制作，值得拥有，手工制作，值得拥有，快点加入购物车吧！'
              }
            ]
          }
      },
      created(){

      },
      methods:{
        go_back(){
          this.$router.push({
            name:'index'
          })
          // window.history.back(-1);
        },
        toCar(){
          var that = this
          that.$router.push({
            name:'car',
          })
        }
      },
      beforeRouteLeave (to, from, next) {
        if (to.name == 'index' && from.name == 'detail') {
          to.meta.isUseCache = true;
        }
        next();
      },
    }
</script>

<style scoped>
  .detail_list img{
    display: inline-block;
    width: 100%;
    vertical-align: top;
  }
  .content_text{
    padding: 10px 5px;
  }
  .detail_img{
    position: relative;
  }
  .detail_back{
    position: absolute;
    display: inline-block;
    width: 30px;
    height: 30px;
    border-radius: 50%;
    background: #aaaaaa;
    opacity: 0.3;
    top: 5px;
    left: 5px;
    color: black;
    text-align: center;
    line-height: 30px;
    /*font-size: 20px;*/
  }
  .primary{
    width: 80%;
    margin-left: 10%;
    margin-top: 10px;
  }
</style>
