<template>
  <div id="app">
	<main>
   		<router-view/>
	</main>
 
    <footer>
    	<nav>
			<router-link tag="div" :to="{name:'Home'}" exact>
				<i class="fa fa-home"></i>
				<span>首页</span>
			</router-link>
			<router-link tag="div" :to="{name:'Category'}">
				<i class="fa fa-bars"></i>
				<span>分类</span>
			</router-link>
			<router-link tag="div" :to="{name:'Cart'}">
				<i class="fa fa-shopping-cart"></i>
				<span>购物车</span>
			</router-link>
			<router-link tag="div" :to="{name:'Me'}">
				<i class="fa fa-o"></i>
				<span>个人中心</span>
			</router-link>
    	</nav>
  </footer>
  </div>
 
</template>

<script>
export default {
  name: 'App'
}
</script>

<style lang="scss">
// $footer-height:44px;
$them-color:rgb(37, 34, 34);
html,body{
	height: 100%;
	font:14px "Microsoft YaHei",sans-serif,Arial;
}
#app{
	height: 100%;
	& > main{
		height: calc(100% - 44px);
		
		& > div{
			height: 100%;
			padding-top: 40px;
			& > header{
				width: 100%;
				height: 40px;
				background-color: $them-color;
				text-align: center;
				color: white;
				position: fixed;
				left: 0; top:0;
				line-height: 40px;
				z-index: 999;
			}
			.content{
				// height: calc(100% - 40px);
				overflow-y: auto; 
				overflow-x: hidden; 
			}
		}
	}
}
footer{
	width: 100%;
	height: 44px;
	position: fixed;;
	left: 0; bottom:0;
	background-color: $them-color;
	z-index: 999;
	nav{
		display: flex;
		div{
			padding: 10px;
			margin: 0 auto;
			color: #fff;
		}
	}
}
</style>