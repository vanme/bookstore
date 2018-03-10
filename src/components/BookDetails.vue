<template>
  <div class="">
    <header>
        <router-link class="leftbtn" :to="{name:'Category'}"><i class="fa fa-angle-left fa-2x"></i></router-link>
        图书详情
        <span class="rightbtn">
        <i class="fa fa-th-list fa-2x"></i>
        </span>
    </header>
    <div class="content">
        <div class="book-conent">
            <div class="bookNews">
                <div class="bookimg">
                <img :src="$route.query.modelInfo.cover">
                </div>
                <div class="booknews">
                    <p><span>作者</span>{{$route.query.modelInfo.author}}</p>
                    <p><span>页数</span>800</p>
                    <p><span>书号</span>B00CBBJS5Y</p>
                    <p><span>出版日期</span>2012年3月1号</p>
                    <p class="sj"><span>售价</span>￥{{$route.query.modelInfo.price}}</p>
                </div>
            </div>
            <div class="name">
                <h1>{{$route.query.modelInfo.name}}</h1>
            </div>
            <div class="bookButton">
                <div class="box">
                    <button>加入购物车</button>
                    <button>立即购买</button>
                </div>
            </div>
        </div>
        <div class="bookjs">
            <span>内容概要</span>
            <div class="bkconent">
                <p>{{$route.query.modelInfo.describe}}</p>
            </div>
        </div>
        <div class="bookjs">
            <span>内容概要</span>
            <div class="bkconent">
                <p>{{$route.query.modelInfo.describe}}</p>
            </div>
        </div>
        <div class="bookjs">
            <span>作者简介</span>
            <div class="bkconent">
                <p>{{$route.query.modelInfo.describe}}</p>
            </div>
        </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: '',
    data() {
      return {
       bookinfo:[]
      }
    },
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
*{
    margin: 0;
    padding: 0;
}
.content{
    width: 100%;
    padding-bottom: 100px;
}
img{
    width: 100%;
    height: 100%;
}
header .leftbtn{
    color: #fff;
    float: left;
    padding-left: 15px;
    text-decoration: none;
    padding-top:5px;
}
header .rightbtn{
    float: right;
    padding-right: 10px;
    padding-top:6px;
}
.book-conent{
    width: 100%;
    height: 300px;
    background: #fff;
    border-bottom: 15px solid #ccc;
}
.bookNews{
    width: 100%;
    height: 170px;
    background: #fff;
    }
.bookimg{
    float: left;
    width: 30%;
    height: 140px;
    padding-top: 10px;
    padding-left: 15px;
}
.booknews{
    float: left;
    width: 60%;
    height: 140px;
    padding-top: 10px;
    padding-left: 15px;
}
.booknews p{
    font-size: 12px;
    margin: 0 0 15px 0;
}
.booknews span{
    color: #706d6d;
    padding-right: 30px;
}
.booknews .sj{
    color: red;
}
.name{
    position: relative;
    width: 100%;
    height: 30px;
}
.name h1{
    display: block;
    font-size: 20px;
    padding-left: 15px;
}
.bookButton{
    height: 55px;
}
.bookButton .box{
    width: 80%;
    height: 35px;
    margin: 0 auto;
}
.bookButton button{
    position: relative;
    display: block;
    height: 35px;
    float: left;
    width: 50%;
    height: 45px;
    top: 20px;
    border: 1px solid #000;
    background: #fff;
    cursor: pointer;
    border-radius: 5px;
}
.bookButton button:hover{
    background: green;
    color: #fff;
}
.bookjs{
    width: 100%;
    margin-top: 10px;
}
.bkconent{
    width: 90%;
    height: 200px;
    margin: 0 auto;
    border: 1px solid #000;
    overflow: hidden;
}
.bookjs span{
    position: relative;
    width: 100px;
    height: 30px;
    top: 15px;
    background: #fff;
    font-size: 20px;
    margin-left: 8%;
    font-weight: 800;
}
.bkconent p{
    padding-top: 25px;
    padding-left: 15px;
    padding-right: 15px;
}
</style>