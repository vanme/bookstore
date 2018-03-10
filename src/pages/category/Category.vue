<template>
  <div class="">
     <header>分类</header>
     <div class="content">
       <div class="suoBox">
         <div class="suo">
           <i class="fa fa-search fa-2x"></i>
           <input class="suoipt" type="text" placeholder="请输入您想查找的图书名称进行搜索" v-model="text" @keyup="search">
           </div>
       </div>
       <div class="book-conent">
         <ul>
           <li v-for="item in showBook" @click="showBookInfo(item)">
             <div class="imgs">
               <img :src="item.cover"> 
             </div>
             <div class="boxright">
              <span class="name">{{item.name}}</span>
              <span class="author">{{item.author}}</span>
              <span class="price">￥{{item.price}}</span>
             </div>
             <span class="rightbtn">></span>
           </li>
         </ul>
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
  </div>
</template>

<script>
  export default {
    name: '',
    data() {
      return {
         //  是否显示图书弹出框
                ifshow: false,
                // 购书数量
                counter : 1,
                //面板数据
                modelInfo: {},
          bookinfo:[],
          text:"",
          showBook:[]
      };
    },
    methods: {
      search() {
        if(this.text == "") {
          this.showBook = this.bookinfo;
        } else {
          this.showBook = this.bookinfo.filter(item => {
            return new RegExp(this.text, "gi").test(item.name);
          })
        }
      },
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
            add(){
                this.counter = Number(this.counter) + 1;
            },
            reduce(){
                if(this.counter >=2){
                    this.counter = Number(this.counter) - 1;
                }
            }
    },
    created() {
        axios.get('../../../static/Data/book-info.json').then((resp) => {
            this.bookinfo = resp.data;
            // 确保进入页面时有内容正常显示
            this.showBook = this.bookinfo;
        }).catch(() => {
            throw new Error('数据请求错误！');
        });
        }
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
.suoBox{
  position: relative;
  width: 100%;
  top: 10px;
}
.suo{
  width: 90%;
  height: 40px;
  border: 1px solid #000;
  margin: 0 auto;
  border-radius: 50px;
}
.suo i{
  position: relative;
  float: left;
  display: block;
  margin-top: 5px;
  padding-left: 20px;
}
.suoipt{
  position: relative;
  display: block;
  float: right;
  width: 80%;
  height: 85%;
  top: 2px;
  border: 0;
  right: 5%;
}
.book-conent{
  position: relative;
  width: 90%;
  margin: 0 auto;
}
.book-conent ul{
  display: block;
  width: 100%;
  margin: 0 auto;
  margin: 0;
  padding: 0;
}
.book-conent li{
  position: relative;
  display: block;
  width: 100%;
  height: 110px;
  top: 15px;
  margin-top: 10px;
  border-bottom: 1px solid #ccc;
  cursor: pointer;
  z-index: 10;
}
.link{
  position: absolute;
  display: block;
  width: 100%;
  height: 100%;
  opacity: 0;
  z-index: 999;
}
.book-conent .imgs{
  float: left;
  width: 30%;
  height: 100px;
}
.book-conent .boxright{
  float: left;
  width: 50%;
  height: 100px;
  margin-left: 15px;
}
.book-conent .boxright span{
  display: block;
  width: 100%;
  height: 20px;
  margin: 10px;
  overflow: hidden;
}
.book-conent .name{
  color: rgb(44, 85, 196);
}
.book-conent .author{
  color: rgb(165, 155, 155);
}
.book-conent .price{
  font-size: 18px;
  color: red;
}
.book-conent .rightbtn{
  display: block;
  float: right;
  line-height: 100px;
  font-size: 20px;
  color: rgb(165, 155, 155);
}
.books{
        position: fixed;
        width: 90%;
        height: 550px;
        border: 2px solid #000;
        background: #fff;
        z-index: 999;
        top:20px;
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
        margin-top:10px;
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
        margin-top: 15px;
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