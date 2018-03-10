<template>
  <div class="paycenter">
      <div class="title">
        <div class="goback" @click="$emit('close')"><</div>
        <p>结算中心</p>
      </div>
      <div class="contents">
      <div class="titleBox">
          <label for="" class="title_2">商品信息确认</label>
      </div>
        <div class="messageBox">
          <div class="messageTitle">价格信息</div>
          <div class="moneyMessage">
            <p class="smallSize">商品总价：<span>{{money}}</span><span>¥</span></p>
            <p class="smallSize">运送费：<span>10</span><span>¥</span></p>
            <p class="total">订单总计：<span>{{money+10}}</span><span>¥</span></p>
          </div>
          <div class="address">
            <div class="messageTitle">收货信息</div>
            <div class="moneyMessage">
                
                <p class="smallSizes">收货地址：<span class="adinfo">四川省&nbsp;成都市&nbsp;高新区&nbsp;景东大道东段拉得方斯东楼10层2号</span></p>
                    
                <p class="smallSizes">收货人：&nbsp;&nbsp;&nbsp;<span class="adinfo">今天不吃鸡</span></p>
                <p class="smallSizes">邮编&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="adinfo">18923213</span></p>
                <p class="smallSizes">电话&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="adinfo">18274715112</span></p>
            </div>
            <div class="chooseaddress" @click="showone=!showone">
                <label for="">收货地址</label>
                <span>></span>
            </div>
            <div class="chooseaddress" @click="showtwo=!showtwo">
                <label for="">新增地址</label>
                <span>></span>
            </div>
          </div>
        <transition name="showout">
            <Changeaddress v-show="showone" @closeone="closeone"/>
        </transition>
        <transition name="showout">
            <Newaddress v-show="showtwo" @closetwo="closetwo"/>
        </transition>    
      </div>
      <div class="paymethod">
        <div class="titleBox">
          <label for="" class="title_2">选择支付方式</label>        
        </div>
        <div class="messageBox">
            <div class="chooseaddress">
                <i></i>
                <label for="">微信支付</label>
                <span>></span>
            </div> 
            <div class="chooseaddress">
                <i></i>
                <label for="">支付宝</label>
                <span>></span>
            </div> 
            <div class="chooseaddress">
                <i></i>
                <label for="">银行卡</label>
                <span>></span>
            </div> 
            <div class="chooseaddress">
                <i></i>
                <label for="">信用卡</label>
                <span>></span>
            </div> 
            <div class="chooseaddress">
                <i></i>
                <label for="">VISA</label>
                <span>></span>
            </div> 
        </div>      
      </div>
      </div>
    </div>
</template>


<script>
    import Changeaddress from '@/components/Changeaddress'
    import Newaddress from '@/components/Newaddress'
    export default{
        name:'Paycenter',
        props: ["money"],
        data () {
            return {
                showone:false,
                showtwo:false
            }
        },
        components:{
            Changeaddress,
            Newaddress
        },
        methods:{
            closeone(){
                this.showone=!this.showone
            },
            closetwo(){
                this.showtwo=!this.showtwo
            }
        }
    }
</script>

<style>
    @import "../style/pay.css";
    .showout-enter-active,.showout-leave-active {
  transition: all .8s;
}

.showout-enter{
  transform: translateY(100%);
}
.showout-leave-to{
    transform: translateX(100%)
}
</style>
