<template>
  <div class="settle">
    <mt-header class="header" title="填写订单">
      <router-link to="" onclick="window.history.back()" slot="left">
        <mt-button icon="back"></mt-button>
      </router-link>
    </mt-header>
    <ul class="userInfo" @click="goAddress">
      <li style="width: 90%" v-for="(value,index) in userInfos" :key="index">
        <div class="info"><span>{{value.name}}</span><span style="margin-left: 30px">{{value.tel}}</span></div>
        <div class="address">{{value.address}}</div>
      </li>
      <span class="icon">
        <i class="fa fa-chevron-right"></i>
      </span>
    </ul>
    <div class="payment" @click="actionSheet"><span>支付方式</span><span>{{payment_text}}</span></div>
    <mt-actionsheet
      :actions= "data"
      v-model="sheetVisible">
    </mt-actionsheet>
    <ul style="border-bottom: 1px solid #eeeeee;padding: 5px 10px;">
      <li class="shopList" v-for="(value,index) in list" :key="index" v-if="index<4">
        <div>
          <img class="shopList_img" :src="value.images" alt="">
          <div class="shopList_content">
            <div class="text">{{value.title}}</div>
            <div class="text" style="font-size: 12px;color: #aaaaaa;margin-top: 2px">{{value.detail}}</div>
            <div style="font-size: 12px;margin-top: 2px;padding-left: 8px;padding-right: 8px;display: flex;justify-content: space-between"><span style="color: red">￥<span style="font-weight: 600;font-size: 14px">{{value.price}} </span><span style="color: orangered" @click="price_message"><i class="fa fa-exclamation-circle"></i></span></span><span>{{value.weight}}0kgx{{value.num}}</span></div>
            <div style="font-size: 12px;margin-top: 2px;padding-left: 8px;" v-if="value.rejected"><span style="border: 1px solid red;border-radius: 12px;padding: 0 8px;color: red">支持七天无理由退货</span></div>
          </div>
        </div>
        <div class="num"><span>购买数量</span><span><button class="btn" @click="lessen(index)">-</button><span style="font-size: 12px;width: 20px;display: inline-block;text-align: center;background: #ddd">{{value.num}}</span><button class="btn" @click="add(index)">+</button></span></div>
        <div class="dispatching">
          <span>配送</span><span>京东快递</span>
        </div>
        <div class="rejected" v-if="value.rejected">
          <div style="width: 75%">
            <div style="font-size: 14px"><span>退换无忧</span><span><span style="font-size: 12px"> ￥</span>{{value.rejected_pic}}.00 </span><span style="color: orangered"><i @click="rejecte_message" class="fa fa-exclamation-circle"></i></span></div>
            <span style="padding: 5px 0;display: inline-block">7天内退货，15天内换货，可享1次上门取件(不另收费)</span>
          </div>
          <div class="switchBox" v-if="value.rejected">
            <mt-switch class="switch" v-model="value.isShow" @change="isOpen"></mt-switch>
          </div>
        </div>
      </li>
    </ul>
    <div class="priceAll">
      <div class="shop_price picFlex"><span>商品金额</span><span>￥{{priceALL}}.00</span></div>
      <div class="freight picFlex"><span>运费(总重:{{weightAll | capitalize}}kg)</span><span class="pic_color">+￥{{freight}}.00</span></div>
      <div class="coupon picFlex" v-if="coupon">优惠券<span class="pic_color">-￥{{coupon}}.00</span></div>
    </div>
    <div class="address_l" v-for="(value,index) in userInfos" :key="index">
      <span style="width: 90%;display: inline-block">{{value.address}}</span>
    </div>
    <div class="bottom">
      <div class="bottom_pic"><span style="font-size: 12px">￥</span>{{priceALL_coupon}}.00</div>
      <div class="bottom_sub" @click="submit">提交订单</div>
    </div>
  </div>
</template>

<script>
    export default {
        name: "settle",
      data(){
          return{
            payment_text:'在线支付',
            data:[{
              name:'在线支付',
              method:this.payments
            },{
              name:'货到付款',
              method:this.goodActive
            },{
                name:'公司付款',
                method:this.firm
              }],
            sheetVisible:false,
            list:[
              {
                title:'陶瓷杯',
                price:100,
                images:'http://cdn.qiqiangkeji.com/20180926.fff7f6be283261b884d7f400be65e1b1_1024x710.jpg',
                detail:'手工制作，值得拥有，快点加入购物车吧！',
                num:2,
                checked:false,
                isShow:false,
                rejected_pic:1,
                weight:1.25,
                rejected:true
              },
              {
                title:'玻璃杯',
                price:200,
                images:'http://cdn.qiqiangkeji.com/20180926.9383d209c5395f8598ca9a2731696a94_850x680.jpg',
                detail:'手工制作，值得拥有',
                num:1,
                checked:false,
                isShow:false,
                rejected_pic:10,
                weight:1.05,
                rejected:true
              },
              {
                title:'高级吸汗毛巾',
                price:10,
                images:'http://cdn.qiqiangkeji.com/20180926.83a59e1a91b31003fc2b2beffd7702b7_320x220.jpg',
                detail:'手工制作，值得拥有，手工制作，值得拥有，快点加入购物车吧！',
                num:1,
                checked:false,
                isShow:false,
                rejected_pic:15,
                weight:0.15,
                rejected:true
              },
              {
                title:'纯棉围巾',
                price:120,
                images:'http://cdn.qiqiangkeji.com/20180926.3015a6dd5205bb599914c7b972f11f04_500x500.jpg',
                detail:'手工制作，值得拥有，手工制作，值得拥有，快点加入购物车吧！',
                num:1,
                checked:false,
                isShow:false,
                rejected_pic:4,
                weight:0.65,
                rejected:true
              },
            ],
            priceALL:0,
            rejected_pic_all:0,
            priceALL_coupon:0,
            priceALL_coupon_a:0,
            freight:0,
            coupon:5,
            weightAll:null,
          }
      },
      computed:{
        userInfos:function() {  //通过方法访问
          return this.$store.getters.id_num;
        },
      },
      created(){
          this.tel();
          this.price();
      },
      methods:{
        tel(){
          for(var i=0;i<this.userInfos.length;i++){
            var str = String(this.userInfos[i].tel)
            this.userInfos[i].tel = str.substr(0,3) + '****' + str.substr(7)
          }
        },
        actionSheet(){
          this.sheetVisible = true;
        },
        payments(){
          this.payment_text = '在线支付'
        },
        goodActive(){
          this.payment_text = '货到付款'
        },
        firm(){
          this.payment_text = '公司付款'
        },
        submit(){
          let instance = this.$toast('等待支付功能的完善');
          setTimeout(()=>{
            instance.close()
          },3000)
        },
        lessen(num){
          if(this.list[num].num>1){
            this.list[num].num = this.list[num].num - 1;
          }
          this.price();
        },
        add(num){
          this.list[num].num = this.list[num].num + 1;
          this.price();
        },
        price(){
          this.priceALL = 0;
          this.weightAll = 0; //总重量
          for(var i =0;i<this.list.length;i++){
            this.priceALL = this.priceALL + this.list[i].price * this.list[i].num;
            this.weightAll = Math.round((this.weightAll + this.list[i].weight * this.list[i].num)*100)/100;
          }
          this.priceALL_coupon = this.priceALL - this.coupon;
          this.priceALL_coupon_a = this.priceALL_coupon;
          this.isOpen();
        },
        //退货保险
        isOpen(){
          this.rejected_pic_all = 0;
          for (var i=0;i<this.list.length;i++){
            if(this.list[i].isShow == true){
              this.rejected_pic_all = this.rejected_pic_all + this.list[i].rejected_pic;
            }
          }
          this.priceALL_coupon = this.priceALL_coupon_a
          this.priceALL_coupon = this.priceALL_coupon + this.rejected_pic_all;
        },
        goAddress(){
          this.$router.replace({
            name:"address_i"
          })
        },
        //价格 message弹框提示
        price_message(){
          this.$messagebox({
            title: '价格说明',
            message: '因可能存在系统缓存、页面更新导致价格变动异常等不确定性情况出现, 商品售价以本结算页商品价格为准;<br><br>如有疑问, 请您立即联系销售商咨询。',
            confirmButtonText:'我知道了',
            confirmButtonClass:'confirm',
          });
        },
        //退货 message弹框提示
        rejecte_message(){
          this.$messagebox({
            title: '退换无忧须知',
            message: '<div style="color: black;margin-bottom: 5px">使用限制规则：</div>' +
            '<div>1.自签收后若7天内退货, 15天内换货时可享受此服务；<br>' +
            '2.自营商品：如符合上门取件, 可享1次上门取件服务, 不在京东取件范围内可享受相应运费补偿, 此服务不影响商品的七天无理由退货政策；<br>' +
            '3.第三方商家商品：因商品品类, 退换地址等不同, 购买价格会有浮动, 购买后如符合赔付条件, 则可享受1次运费赔付或由京东配送提供1次免费退换货配送服务, 运费赔付最高限额25元/单, 赔付到小金库账户余额。</div>' +
            '<div style="color: black;margin: 5px 0">特殊说明：</div><div>一、自营商品：<br>' +
            '1.如果您在购买京东自营商品时选择了退换无忧服务, 那么该订单在使用期限内可以享受......</div>',
            confirmButtonText:'我知道了',
            confirmButtonClass:'confirm',
          });
        },
      },
      //过滤器
      filters:{
        capitalize:function (value) {
          if(String(value).length - (String(value).indexOf('.') + 1)==1){
            if(String(value).indexOf('.') <=0){
              return value = String(value) + '.000';
            }else {
              return value = String(value) + '00'
            }
          }else if(String(value).length - (String(value).indexOf('.') + 1)==2){
            if(String(value).indexOf('.') <=0){
              return value = String(value) + '.000'
            }else {
              return value = String(value) + '0'
            }
          }
        }
      },
    }
</script>

<style scoped>
  .shopList{

  }
  .shopList_img{
    display: inline-block;
    vertical-align: middle;
    width: 30%;
    height: 70px;
  }
  .dispatching{
    display: flex;
    justify-content: space-between;
    font-size: 14px;
    padding: 5px 0px;
  }
  .dispatching span{
    display: block;
  }
  .dispatching span:nth-of-type(2){
    margin-right: 10px;
  }
  .rejected{
    font-size: 12px;
    /*margin-top: 10px;*/
    display: flex;
    justify-content: space-between;
    /*padding: 5px 10px;*/
  }
  .payment{
    border-bottom: 1px solid #eeeeee;
    padding: 5px 10px;
    display: flex;
    justify-content: space-between;
    font-size: 14px;
    height: 25px;
    line-height: 25px;
  }
  .userInfo{
    font-size: 14px;
    display: flex;
    justify-content: space-between;
    padding: 5px 10px;
    border-bottom: 1px solid #eeeeee;
    margin-top: 40px;
  }
  .icon{
    display: inline-block;
    display: flex;
    align-items: center;
  }
  .info{
    padding: 5px 0;
  }
  .address{
    font-size: 12px;
    color: #aaaaaa;
  }
  .switchBox{
    display: flex;
    align-items: center;
  }
  .pic_color{
    color: orangered;
  }
  .picFlex{
    display: flex;
    justify-content: space-between;
    padding-bottom: 5px;
  }
  .priceAll{
    padding: 5px 10px 130px 10px;
    font-size: 14px;
  }
  .bottom{
    position: fixed;
    bottom: 0;
    height: 40px;
    width: 100%;
    display: flex;
    text-align: center;
    line-height: 40px;
  }
  .bottom_pic{
    background: orangered;
    width: 60%;
    height: 40px;
    text-align: left;
    text-indent: 3px;
    color: red;
  }
  .bottom_sub{
    background: red;
    width: 40%;
    color: white;
    height: 40px;
  }
  .header{
    background: orangered;
    position: fixed;
    top: 0;
    width: 100%;
    z-index: 99999;
  }
  .address_l{
    position: fixed;
    bottom: 40px;
    width: 100%;
    background: #FFDEAD;
    padding: 5px 8px;
    text-align: left;
    font-size: 12px;
    color: 	#DEB887;
  }
  .num{
    display: flex;
    justify-content: space-between;
    font-size: 14px;
    margin-top: 10px;
  }
  .btn{
    border: none;
    outline: none;
    background: white;
    font-size: 14px;
  }
  .shopList_content{
    display: inline-block;
    font-size: 14px;
    width: 68%;
    vertical-align: middle;
  }
  .text{
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    padding-left: 8px;
  }
</style>
