<template>
  <div class="personcenter">
      <header>
        <i class="fa fa-angle-left backbtn" v-show="showback" @click="back"></i>
        <i class="fa fa-angle-left backbtn" v-show="showback2" @click="back2"></i>
        {{logtitle}}
      </header>
      <div class="content">
        <!-- 登录注册页面 -->
        <div class="notlog" v-show="notlogstates">
          <div class="title">
            <div :class="logfontcolor" @click="logtitlebtn">登录</div>
            <div @click="registbtn" :class="regsitfontcolor">注册</div>
          </div>
          <div class="telinput">
            <i class="fa fa-mobile-phone"></i>
            <input type="number" placeholder="请输入您的手机号" v-model="telval">
          </div>
           <div class="nicknameinput" v-show="shownickname">
            <i class="fa fa-user"></i>
            <input type="text" placeholder="请输入您的昵称" v-model="nicknameval">
          </div>
          <div class="pwdinput">
            <i class="fa fa-lock"></i>
            <input type="password" placeholder="请输入您的密码" v-model="pwdval">
          </div>
           <div class="confirmpwdinput" v-show="showconfirmpwd">
            <i class="fa fa-lock"></i>
            <input type="password" placeholder="请确认您的密码" v-model="confirpwdval">
          </div>
          <p class="wrongcue" v-show="showwrongcue">未注册或手机或密码输入错误！</p>
          <button @click="logon" class="logbtn" v-show="showlogonbtn">登录</button>
          <button @click="regist" class="logbtn" v-show="showregistbtn">注册</button>
          <section class="mask" v-show="showmask">
            <div>
              <p>注册成功!</p>
              <button @click="maskbtn">确认</button>
              <button @click="maskbtn">取消</button>
            </div>
          </section>
        </div>
        <!-- 登录成功后个人中心页面 -->
        <div class="logsuceess" v-show="logsucessstates">
        <section class="personimgcont"></section>
        <div class="personimgcontdiv">
           <div class="personimg"></div>
           <p class="personname">{{personname}}</p>
        </div>
        <ul class="personinfo">
          <li @click="topersondata"><a href="javascript:void(0)" >设置个人资料</a></li>
          <li @click="toaddress"><a href="javascript:void(0)">收货地址管理</a></li>
        </ul>
        <ul class="mycomment">
          <li><a href="javascript:void(0)">我的订单</a></li>
          <li><a href="javascript:void(0)">我的评价</a></li>
        </ul>
        <ul class="collection">
          <li><a href="javascript:void(0)">商品收藏</a></li>
          <li><a href="javascript:void(0)">店铺收藏</a></li>
          <li><a href="javascript:void(0)">浏览记录</a></li>
        </ul>
        <div class="outlogbutton">
          <button type="button" @click="logoff">退出登录</button>
        </div>
      
      </div>
        <transition name="slide-fade">
          <PersonerData v-show="showpersonerdata" @showdatachange= "showdatachanges"></PersonerData>
        </transition>
        <transition name="slide-fade">
         <Changeaddress v-show="showaddress"></Changeaddress>
        </transition>
      </div>
  </div>
</template>

<script>
  import PersonerData from "../../components/PersonerData.vue";
  import Changeaddress from "../../components/Changeaddress.vue"
  export default {
    name: 'Me',
    components:{
        PersonerData,
        Changeaddress
    },
    data() {
            return {
              logtitle:'登录/注册',
              logsucessstates: false,
              notlogstates: true,
              showwrongcue:false,
              telval:'',
              pwdval:'',
              nicknameval:'',
              confirpwdval:'',
              logfontcolor:{
                'fontgreen':true,
              },
              regsitfontcolor:{
                'fontgreen':false
              },
              shownickname:false,
              showconfirmpwd:false,
              showlogonbtn:true,
              showregistbtn:false,
              showmask:false,
              personname:'',
              showpersonerdata:false,
              showback:false,
              showaddress:false,
              showback2:false
            }
        },
    methods:{
      //点击注册标题按钮
      registbtn(){
         this.logfontcolor.fontgreen =  false;
         this.regsitfontcolor.fontgreen = true;
         this.shownickname = true;
         this.showconfirmpwd = true;
         this.showregistbtn = true;
         this.showlogonbtn = false,
         this.showwrongcue = false;
      },
      //点击登录标题按钮
      logtitlebtn(){
         this.logfontcolor.fontgreen =  true;
         this.regsitfontcolor.fontgreen = false;
         this.shownickname = false;
         this.showconfirmpwd = false;
         this.showlogonbtn = true,
         this.showregistbtn = false;
         this.showwrongcue = false;
      },
       //确认注册
      regist(){
       //验证是否手机格式
        var reg1 = /^1(3|4|5|7|8)\d{9}$/;
        var registtelresult = reg1.test(this.telval);
        //验证是否为密码格式
        var reg2 = /^[a-zA-Z]\w{5,17}$/;
        var registpwdresult = reg2.test(this.pwdval);
        //验证两次密码是否一样
       //如果全部验证通过，则注册成功
       if(registtelresult && registpwdresult && (this.pwdval === this.confirpwdval)){
         this.showmask = true;
         //存储值
         localStorage.setItem('telval',this.telval);
         localStorage.setItem('nickname',this.nicknameval);
         localStorage.setItem('pwdval',this.pwdval);
         //清空输入框
          this.telval = '';
          this.pwdval = '';
          this.nicknameval = '';
          this.confirpwdval = '';
       }
       else{
         //输入错误，弹出提示消息并且清空原来的值
          this.showwrongcue = true;
          this.telval = '';
          this.pwdval = '';
          this.nicknameval = '';
          this.confirpwdval = '';
       }
      },
      //确认登录
      logon(){
        //验证是否登录成功
        //如果登录不成功，则显示登录失败提示语
        //验证手机号码是否为注册的手机号
        var telvalregist = localStorage.getItem('telval');
        var pwdvalregist = localStorage.getItem('pwdval');
        if(telvalregist === this.telval && pwdvalregist === this.pwdval){
          //登录成功显示个人中心页面
          this.logtitle = '个人中心';
          this.notlogstates = false;
          this.logsucessstates = true;
          this.showwrongcue = false ;
          this.personname = localStorage.getItem('nickname');
          //清空原来的值
          this.telval = "";
          this.pwdval = ""
        }
        else{
          this.showwrongcue = true;
          this.telval = "";
          this.pwdval = ""
        }
      },
      //退出登录
      logoff(){
        this.logtitle = '登录/注册';
        this.logsucessstates = false;
        this.notlogstates = true;
      },
      maskbtn(){
        this.showmask = false;
        this.showwrongcue = false;
      },
      //点击个人资料事件
      topersondata(){
        this.logtitle = '个人资料修改';
        this.showback = true;
        this.logsucessstates = false;
        this.showpersonerdata = true;
      },
     //子组件事件
     showdatachanges(){
       this.showpersonerdata = false;
       this.logsucessstates = true;
       this.showback = false;
       this.personname = localStorage.getItem('nickname');
     },
     back(){
       this.showpersonerdata = false;
       this.logsucessstates = true;
       this.showback = false;
     },
     //点击收货地址
     toaddress(){
       this.showaddress = true;
       this.logtitle = '个人资料修改';
       this.showback2 = true;
     },
     back2(){
        this.showaddress = false;
         this.showback2 = false;
     }

    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>

// 初始化样式
html,body,ul,li{
  margin: 0;
  padding: 0;
  width: 100%;
}
.content{
  margin-bottom: 40px;
}
ul,li{
  list-style: none;
  background-color: #fff;
}
a{
  text-decoration: none;
}
.personcenter{
  font-family: '微软雅黑';
  color: white;
  font-size: 14px;
}
button:hover{
  opacity: 0.9;
}
//header部分
.personcenter>header{
  background-color: #000;
  color: white;
  height: 45px;
  line-height: 45px;
  text-align: center;
  position: relative;
}
.backbtn{
  position: absolute;
  left: 10px;
  font-size: 35px;
  vertical-align: middle;
}
//登录页面
.notlog{
  width: 100%;
  // padding: 0 10px;
  color: #000;
  font-size: 18px;
  background-color: #fff;
}
.notlog>div{
  height: 55px;
  line-height: 55px;
  border-bottom: 1px solid rgb(174, 174, 174);
  margin-bottom: 50px;
}
//登录/注册标题
.title{
  display: flex;
  margin-bottom: 60px;
}
.title>div{
  width: 50%;
  text-align: center;
  letter-spacing: 8px;
}
.fontgreen{
  color:rgb(51, 153, 51);
  border-bottom: 5px solid rgb(51, 153, 51);
}
.fontbalck{
  color: #000;
}
// 登录输入框和密码框
.telinput>i,.pwdinput>i,.nicknameinput>i,.confirmpwdinput>i{
  display: inline-block;
  width: 26px;
  font-size: 32px;
  vertical-align: middle; 
  color: rgba(102,102,102,1);
}
.telinput>input,.pwdinput>input,.nicknameinput>input,.confirmpwdinput>input{
   width: 200px;
   vertical-align: middle; 
   height: 35px;
   outline: none;
   border: none;
   padding-left: 15px;;
  
}
// 登录按钮
.logbtn{
  width: 100%;
  height: 50px;
  border: none;
  outline: none;
  border-radius: 5px;
  background-color: rgba(51,153,51,1);
  color: #fff;
  margin-bottom: 40px;
}
.wrongcue{
  color: rgb(236, 38, 38);
}
//注册成功的遮罩层
.mask{
  width: 100%;
  height: 100%;
  background: rgba(0,0,0,0.4);
	position: fixed;
	top: 0;left: 0;
	color: white; 
}
.mask>div{
  width: 300px;
  height: 200px;
  text-align: center;
  margin: 200px auto;
  background-color: #fff;
  border: 1px solid #eee;
  color: #000;
}
.mask button{
  width: 100px;
  height: 35px;
  border: none;
  outline: none;
  background-color: rgba(51,153,51,1);
  color: #fff;
  margin-top: 30px;
}
//登录成功后个人中心页面
.logsuceess{
   background-color: rgba(242,242,242,1);
   position: relative;
}
//个人中心的头像图片部分
.personimgcont{
  width: 100%;
  height: 195px;
  background: rgba(238,255,255,1) url(../../../static/imgs/1.jpg) no-repeat center;
  opacity: .3;
}
.personimgcontdiv{
  width: 100%;
  position: absolute;
  top:0px;
  overflow: hidden;
  text-align: center;
}
.personimg{
  width: 120px;
  height: 120px;
  margin: 15px auto;
  border-radius: 50%;
  background-image: url(../../../static/imgs/1.jpg);
  background-position: center;
  background-size: cover;
  
}
.personname{
  font-size: 18px;
  color: rgba(204, 102, 127, 1);
}
//个人资料，收货地址等部分
ul{
  margin-top: 15px;
  padding: 0 15px;
}
ul>li{
  height: 50px;
  line-height: 50px;
  border-top: 1px solid rgba(215,215,215,1);
}
ul>li>a{
  color: rgba(94,94,94,1);
}
//退出登录按钮
.outlogbutton{
   width: 100%;
  //  padding: 0 15px;
   margin: 20px 0;
}
.outlogbutton>button{
  width: 100%;
  height: 50px;
  line-height:50px;
  outline: none;
  border: 0;
  background-color: rgba(204,51,51,1);
  color: white;
  border-radius: 3px;
  text-align: center;
}
// 动画切换效果
.slide-fade-enter-active {
  transition: all .3s ease;
}
.slide-fade-leave-active {
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active for below version 2.1.8 */ {
  transform: translateX(10px);
  opacity: 0;
}
</style>