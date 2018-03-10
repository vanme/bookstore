<template>
<div id="persondata" v-show="showpersondatacomp">
    <div>
        <span>昵称</span>
        <input type="text" v-model="changenickname">
    </div>
     <div>
        <span>E-mail</span>
        <input type="email" placeholder="请填写您的安全邮箱" v-model="changeemail">
    </div>
     <div class="choosegender">
        <span>性别</span>
        <div v-for="(gender,index) in genders">
            <div class="genderchoose" :class="{checked:gender.states}" @click="checkItem(index)">{{gender.name}}</div>
        </div> 
    </div>
    <div>
        <span>生日</span>
        <input type="date" v-model="changebirth">
    </div>
    <div>
        <span>身份证号</span>
        <input type="text" placeholder="请输入您的身份证号" v-model="changeid">
    </div>
    <div id="medescipbediv">
        <span>个人描述</span>
        <textarea name="medescipbe" v-model="changedes"></textarea>
    </div>
    <p v-show="showemialwrong">邮箱或身份证格式错误</p>
    <button @click="confirmchangeper">确认修改</button>
    <section class="mask" v-show="showchangemask">
        <div>
            <p>修改成功!</p>
            <button @click="changemaskbtn">确认</button>
        </div>
    </section>
</div>
    
</template>

<script>
export default {
  name:'PersonerData',
  data(){
     return{
         showpersondatacomp:true,
         showchangemask:false,
         changenickname:"",
         changeemail:"",
         changegender:"",
         changebirth:"1995-02-04",
         changeid:"",
         changedes:"",
         checkstates:true,
         genders:[
             {name:'男',states:true},
             {name:'女',states:false},
             {name:'保密',states:false},
         ],
         showemialwrong:false
     }
  },
  methods:{
      confirmchangeper(){
          //验证是否为邮箱格式和身份证格式
          var reg1 = /^\w+@[a-z0-9]+\.([a-z]){1,3}$/;
          var reg2 = /^[1-9]\d{14}|^[1-9]\d{17}|^[1-9]\d{16}x/;
          var emailresult = reg1.test(this.changeemail);
          var idresult = reg2.test(this.changeid);
          if(emailresult && idresult){
              this.showchangemask = true;
          }
          else{
              this.showemialwrong = true;
          }
          
      },
      changemaskbtn(){
        //   this.showpersondatacomp = false;
          this.showemialwrong = false;
          this.showchangemask = false;
          localStorage.setItem('nickname',this.changenickname);
          localStorage.setItem('email',this.changeemail);
          localStorage.setItem('gender',this.changegender);
          localStorage.setItem('birth',this.changebirth);
          localStorage.setItem('id',this.changeid);
          localStorage.setItem('des',this.changedes);
          this.$emit('showdatachange');
      },
      checkItem(index){
          const genderleng = this.genders.length;
          for(var i = 0;i < genderleng;i++){
              if( i != index){
                  this.genders[i].states = false;
              }
              else{
                  this.genders[i].states = true;
              }
          }
      }
    
  }
}
</script>

<style lang="scss" scoped>
//初始化样式
    html,body {
        height: 100%;
        color: #000;
        font: 16px "Microsoft YaHei",sans-serif,Arial;
    }
    #persondata{
        width: 100%;
        padding: 0 15px;
        & > .mask{
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.4);
	        position: fixed;
	        top: 0;left: 0;
	        color: #000;
            & > div{
                width: 300px;
                height: 200px;
                text-align: center;
                margin: 200px auto;
                background-color: #fff;
                border: 1px solid #eee;
              
                & > p {
                    margin-top: 20px;
                    font-size: 18px;
                }
                & > button{
                    width: 100px;
                    height: 35px;
                    border: none;
                    outline: none;
                    background-color: rgba(51,153,51,1);
                    color: #fff;
                    margin-top: 30px;
                }
            }

        }
        & > div#medescipbediv{
            text-align: left;
            height: 100px;
        }
        & > div.choosegender{
            text-align: left;
            span{
                margin-left: 28px;
                margin-right: 50px;
            }
           & > div{
                display: inline-block;  
            }
            
        }
        & > div{
            color: #000;
            height: 55px;
            line-height: 55px;
            border-bottom: 1px solid rgba(174,174,174,1);
            text-align: right;
                div.genderchoose{
                    width: 50px;
                    height: 30px;
                    line-height: 30px;
                    text-align: center;
                    border: 1px solid #ddd;
                    border-radius: 5px;
                    cursor: pointer;
                    margin: 0 4px;
                    display: inline-block;
                    &.checked{
                        background-color: rgba(51,153,51,1);
                        color: #fff;
                    }
                }
            span{
                height: 30px;
                color: rgba(152,152,152,1);
            }
            input{
                width: calc(100% - 114px);
                height: 30px;
                margin-left:50px;
                outline: none;
                border: none;
            }
            textarea{
                margin-top: 10px;
                margin-left: 50px;
                vertical-align:top;
                height: 80px;
                width: 200px;
                outline: none;
                border: none;
            }
        }
        & > p{
            margin-top: 20px;
            color: rgb(236, 38, 38);
        }
        & > button{
            width: 100%;
            height: 50px;
            // margin-top: 10px;
            color: #fff;
            background-color: rgba(53, 154, 53, 1);
            outline: none;
            border: none;
            border-radius: 5px;
        }

    }
</style>
