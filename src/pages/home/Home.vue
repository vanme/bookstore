<template>
  <div class="">
      <header>掌上书城</header>
      <div class="content">
         <div class="swiper-container">
            <div class="swiper-wrapper">
                <div class="swiper-slide"><img :src="imgArray[0]"></div>
                <div class="swiper-slide"><img :src="imgArray[1]"></div>
                <div class="swiper-slide"><img :src="imgArray[2]"></div>
            </div>
            <!-- Add Pagination -->
            <div class="swiper-pagination"></div>
            <!-- Add Arrows -->
            <div class="swiper-button-next"></div>
            <div class="swiper-button-prev"></div>
        </div>
        <div class="active">
            <div>
          <i class="fa fa-bell logon1"></i>
            </div>
          <div>
          <span>双12全场图书低至1折，更有隐藏神秘惊喜!</span>
          </div>
        </div>
        <div class="new-book">
          <h1>新书上架</h1>
          <ul>
              <li v-for="newbook in newbook" @click="showBookInfo(newbook)">
                  <div class="imgs">
                     <img :src="newbook.cover">
                  </div>
                  <span class="name">{{newbook.name}}</span>
                  <span class="price">￥{{newbook.price}}</span>
              </li>
          </ul>
        </div>
        <div class="new-book">
            <h1>编辑推荐</h1>
          <ul>
              <li v-for="groombook in groombook" @click="showBookInfo(groombook)">
                  <div class="imgs">
                     <img :src="groombook.cover">
                  </div>
                  <span class="name">{{groombook.name}}</span>
                  <span class="price">￥{{groombook.price}}</span>
              </li>
          </ul>
        </div>
        <div class="new-book">
            <h1>热门新书</h1>
          <ul>
              <li v-for="hotbook in hotbook" @click="showBookInfo(hotbook)">
                  <div class="imgs">
                     <img :src="hotbook.cover">
                  </div>
                  <span class="name">{{hotbook.name}}</span>
                  <span class="price">￥{{hotbook.price}}</span>
              </li>
          </ul>
        </div>
        <div class="books" v-show="ifshow">
            <div class="bookShoptop">
                <span class="Xo">
                <i class="fa fa-times" @click="closseModal"></i>
                </span>
            </div>
            <div class="bookNews">
                <div class="book-conent">
                    <div class="bookimg">
                        <img :src="modelInfo.cover">
                     </div>
                    <div class="booknews">
                        <p><span>作者</span>{{modelInfo.author}}</p>
                        <p><span>页数</span>800</p>
                        <p><span>书号</span>B00CBBJS5Y</p>
                        <p><span>出版日期</span>2012年3月1号</p>
                        <router-link class="ra" :to="{name:'BookDetails',query:{modelInfo}}">详情 ></router-link>
                    </div>
                </div>
            </div>
            <div class="bookAbout">
                <h1>{{modelInfo.name}}</h1>
                <p>{{modelInfo.describe}}</p>
            </div>
            <div class="bookNumber">
                    <div class="pnfs">
                    <span class="fs">￥{{modelInfo.price}}</span>
                    </div>
                    <div class="pncalc">
                        <span class="calc ca" @click="reduce()"> - </span>
                        <span class="calc"> {{counter}}  本 </span>
                        <span class="calc ca" @click="add()"> + </span>
                    </div>
            </div>
            <div class="bookButton">
                <div class="box">
                    <button @click="addBookList">加入购物车</button>
                    <button>立即购买</button>
                </div>
            </div>
        </div>
      </div>
  </div>
</template>

<script>
  import 'swiper/dist/css/swiper.css'
  import Swiper from 'swiper'
  import axios from 'axios'

//轮播图
    export default {
        name: "swiper",
        data() {
            return {
                //  是否显示图书弹出框
                ifshow: false,
                // 购书数量
                counter : 1,
                //面板数据
                modelInfo: {},
                newbook:[],
                groombook:[],
                hotbook:[],
                imgArray: [
                  '../../../static/imgs/imgs1.jpg',
                  '../../../static/imgs/imgs2.jpg',
                  '../../../static/imgs/imgs3.jpg',
                ],
                localBookList:[],
               counter:"1"
            };
        },
        methods: {
            //加入购物车
            addBookList() {
                // 本页加载的新书
                let newBook = {
                    name: this.modelInfo.name,
                    img: this.modelInfo.cover,
                    author: this.modelInfo.author,
                    price: this.modelInfo.price
                };
                if(!localStorage.book){
                    this.localBookList.push(newBook);
                    localStorage.book = JSON.stringify(this.localBookList);
                }else{
                    // 判断这本书是否已经加入过购物车了
                    // 从本地取出书的数组
                    let localBookArr = JSON.parse(localStorage.book),
                        localBookArr_len = localBookArr.length;
                    for(let i = 0;i < localBookArr_len;i++){
                        // 如果已经加入过购物车了
                        if(newBook.name === localBookArr[i].name){
                            alert ("你已经将我加入过购物车了哦>.<");
                            return;
                        }
                    }
                    localBookArr.push(newBook);
                    localStorage.book = JSON.stringify(localBookArr);
                }
            },
            showBookInfo(book) {
                this.ifshow = true;
                this.modelInfo = {
                    name: book.name,
                    cover: book.cover,
                    describe: book.describe,
                    author: book.author,
                    price: book.price
                }
            },
            closseModal() {
                this.ifshow = false;
            },
            runSwiper() {
                var swiper = new Swiper('.swiper-container', {
                    spaceBetween: 30,
                    centeredSlides: true,
                    autoplay: {
                        delay: 2500,
                        disableOnInteraction: false,
                    },
                    pagination: {
                        el: '.swiper-pagination',
                        clickable: true,
                    },
                    navigation: {
                        nextEl: '.swiper-button-next',
                        prevEl: '.swiper-button-prev',
                    },
                });
            },
            add(){
                this.counter = Number(this.counter) + 1;
            },
            reduce(){
                if(this.counter >=2){
                    this.counter = Number(this.counter) - 1;
                }
            }
        },
        mounted() {
            this.runSwiper();
        },
        //请求访问数据
        created() {
        axios.get('../../../static/Data/newbook.json').then((resp) => {
            this.newbook = resp.data;
        }).catch(() => {
            throw new Error('数据请求错误！');
        });
        axios.get('../../../static/Data/groombook.json').then((resp) => {
            this.groombook = resp.data;
        }).catch(() => {
            throw new Error('数据请求错误！');
        });
        axios.get('../../../static/Data/hotbook.json').then((resp) => {
            this.hotbook = resp.data;
        }).catch(() => {
            throw new Error('数据请求错误！');
        });
        }
    };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
    .content{
    width: 100%;
    padding-bottom: 100px;
    }
    .swiper-container {
        width: 100%;
        height: 100%;

    }

    .swiper-slide {
        height: 170px;
        text-align: center;
        font-size: 18px;
        background: #fff;

        /* Center slide text vertically */
        display: -webkit-box;
        display: -ms-flexbox;
        display: -webkit-flex;
        display: flex;
        -webkit-box-pack: center;
        -ms-flex-pack: center;
        -webkit-justify-content: center;
        justify-content: center;
        -webkit-box-align: center;
        -ms-flex-align: center;
        -webkit-align-items: center;
        align-items: center;
    }
    img{
      width: 100%;
      height: 100%;
    }
    .active{
      width: 100%;
      height: 35px;
      border-bottom: 1px solid #ccc;
    }
    .active i{
      color: orange;
      padding-top: 10px;
      padding-left: 20px;
    }
    .active span{
      width: 100%;
      height: 100%;
      font-size: 15px;
      padding-left: 5px;
    }
    .active div{
        float: left;
        line-height: 35px;
    }
    .new-book{
        position: relative;
        width: 100%;
    }
    .new-book h1{
        display: block;
        width: 100%;
        height: 20px;
        padding-left: 30px;
        font-size: 18px;
        margin: 10px 0 20px 0;
        float: left;
    }
    .new-book ul{
        display: block;
        width: 90%;
        height: 90%;
        margin: 0 auto;
        padding-left: 10px;
    }
    .new-book li{
        display: block;
        float: left;
        width: 48%;
        margin: 5px 0 5px 0;
        text-align: center;
        cursor: pointer;
    }
    .new-book .imgs{
        width: 140px;
        height: 170px;
        margin: 0 auto;
    }
    .new-book .name{
        display: block;
        height: 20px;
        font-size: 15px;
        overflow: hidden;
        //文字超出部分省略
        text-overflow: ellipsis;
        white-space: nowrap;
    }
    .new-book .price{
        display: block;
        color: orangered;
        font-size: 17px;
    }
    .books{
        position: fixed;
        width: 90%;
        height: 550px;
        border: 2px solid #000;
        background: #fff;
        z-index: 9999;
        margin: -230px 5% 0 5%;
    }
    .books .bookShoptop{
        width: 100%;
        height: 35px;
        border-bottom: 1px solid #ccc;
    }
    .bookShoptop .Xo{
        display: block;
        width: 50px;
        line-height: 25px;
        font-size: 20px;
        text-align: center;
        float: right;
        margin-top: 5px;
        cursor: pointer;
    }
    .bookNews{
        width: 100%;
        height: 140px;
        background: #fff;
    }
    .books .bookimg{
        float: left;
        width: 30%;
        height: 140px;
        padding-top: 10px;
        padding-left: 15px;
    }
    .books .booknews{
        float: left;
        width: 60%;
        height: 140px;
        padding-left: 15px;
    }
    .books .booknews p{
        font-size: 12px;
    }
    .books .booknews span{
        color: #706d6d;
        padding-right: 30px;
    }
    .books .booknews .ra{
        color: #706d6d;
        float: right;
        margin-right: 18px;
        margin-bottom: 0;
        cursor: pointer;
        text-decoration: none;
    }
    .bookAbout{
        width: 90%;
        height: 220px;
        margin: 0 auto;
        overflow: hidden;
    }
    .bookAbout h1{
        font-size: 17px;
        padding-top: 10px;
        padding-left: 15px;
        padding-right: 18px;
    }
    .bookAbout p{
        font-size: 13px;
        padding-top: 10px;
        padding-left: 15px;
        padding-right: 15px;
        overflow: hidden;
    }
    .bookNumber{
      height: 60px;
      font-size: 20px;
    }
    .bookNumber .pnfs{
        float: left;
        width: 50%;
        height: 50px;
        border-top:2px solid #ccc;
        border-bottom: 2px solid #ccc;
        border-right: 2px solid #ccc;
        line-height: 50px;
    }
    .pnfs .fs{
        display: block;
        width: 30%;
        margin: 0 auto;
    }
    .bookNumber .pncalc{
        float: left;
        width: 49%;
        height: 50px;
        border-top:2px solid #ccc;
        border-bottom: 2px solid #ccc;
        line-height: 50px;
    }
    .pncalc .calc{
        display: block;
        float: left;
        width: 30%;
        margin: 0 auto;
        text-align: center;
    }
    .pncalc .ca{
        cursor: pointer;
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
        top: 10px;
        border: 1px solid #000;
        background: #fff;
        cursor: pointer;
    }
    .bookButton button:hover{
        background: green;
        color: #fff;
    }
</style>