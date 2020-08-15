<template>
	<div id="detail">
		<detail-nav-bar class="detail-nav" @titleClick="titleClick" ref="nav"></detail-nav-bar>
		<scroll class="content" 
					  ref="scroll" 
						:probe-type="3" 
						@scroll="contentScroll">
			<detail-swiper :top-images="topImages"></detail-swiper>
			<detail-base-info :goods="goods"></detail-base-info>
			<detail-shop-info :shop="shop"></detail-shop-info>
			<detail-goods-info :detail-info="detailInfo" @imageLoad="imageLoad"></detail-goods-info>
			<detail-param-info ref="params" :param-info="paramInfo"></detail-param-info>
			<detail-comment-info ref="comment" :comment-info="commentInfo"></detail-comment-info>
			<goods-list ref="recommend" :goods="recommends"></goods-list>
		</scroll>
		<detail-bottom-bar @addToCart="addToCart"></detail-bottom-bar>
		<back-top @click.native="backClick" v-show="isShowBackTop"></back-top>
		<!-- <toast :message="message" :isshow="isshow"></toast> -->
	</div>
</template>

<script>
	import DetailNavBar from './childComps/DetailNavBar'
	import DetailSwiper from './childComps/DetailSwiper'
	import DetailBaseInfo from './childComps/DetailBaseInfo'
	import DetailShopInfo from './childComps/DetailShopInfo'
	import DetailGoodsInfo from './childComps/DetailGoodsInfo'
	import DetailParamInfo from './childComps/DetailParamInfo'
	import DetailCommentInfo from './childComps/DetailCommentInfo'
	import DetailBottomBar from './childComps/DetailBottomBar'
	
	import Scroll from 'components/common/scroll/Scroll'
	import GoodsList from 'components/content/goods/GoodsList'
	
  import {getDetil,Goods,Shop,GoodsParam,getRecommend} from "network/detail"
	import {debounce} from 'common/utils'
	import {itemListenerMixin,backTopMixin} from 'common/mixin'
	
	import {mapActions} from 'vuex'
	
	// import Toast from 'components/common/toast/Toast'
	
	export default{
		name:"Detail",
		components:{
			DetailNavBar,
			DetailSwiper,
			DetailBaseInfo,
			DetailShopInfo,
			DetailGoodsInfo,
			DetailParamInfo,
			DetailCommentInfo,
			DetailBottomBar,
			Scroll,
			GoodsList,
			// Toast
		},
		mixins:[itemListenerMixin,backTopMixin],
		data(){
			return {
				iid:null,
				topImages:[],
				goods:{},
				shop:{},
				detailInfo:{},
				paramInfo:{},
				commentInfo:{},
				recommends:[],
				itemImgListener:null,
				themeTopYs:[],
				getThemeTopY:null,
				currentIndex:0,
				// message:'',
				// isshow:false
			}
		},
		created(){
			//1.保存传入的iid
			this.iid = this.$route.params.iid
			//2.根据iid请求详情数据
			getDetil(this.iid).then(res => {
				// console.log(res);
				//1.获取数据
				const data = res.result;
				
				//2.取出轮播图的数据
				this.topImages = data.itemInfo.topImages;
				
				//3.创建商品的对象
				this.goods = new Goods(data.itemInfo,data.columns,data.shopInfo.services)
				
				//4.取出店铺的信息
				this.shop = new Shop(data.shopInfo)
				
				//5.取出详情的信息
				this.detailInfo = data.detailInfo
				
				//6.取出参数的信息
				// this.paramInfo = new GoodsParam(data.itemParams.info,data.itemParams.rule)
				this.paramInfo = data.itemParams
				
				//7.取出评论的信息
				if(data.rate.cRate !== 0){
					this.commentInfo = data.rate.list[0]
				}
				
				/*
				//1.第一次获取，值不对。
				//原因：this.$refs.params.$el压根没有渲染
				this.themeTopYs = []
				this.themeTopYs.push(0)
				// this.themeTopYs.push(参数的offsettop)
				this.themeTopYs.push(this.$refs.params.$el.offsetTop)
				this.themeTopYs.push(this.$refs.comment.$el.offsetTop)
				this.themeTopYs.push(this.$refs.recommend.$el.offsetTop)
				console.log(this.themeTopYs);
				
				//2.第二次获取，值不对。
				//原因：图片没有计算在内
				this.$nextTick(() => {
					this.themeTopYs = []
					this.themeTopYs.push(0)
					// this.themeTopYs.push(参数的offsettop)
					this.themeTopYs.push(this.$refs.params.$el.offsetTop)
					this.themeTopYs.push(this.$refs.comment.$el.offsetTop)
					this.themeTopYs.push(this.$refs.recommend.$el.offsetTop)
					console.log(this.themeTopYs);
				})
				*/
			})
			//3.请求推荐数据
			getRecommend().then(res => {
				// console.log(res);
				this.recommends = res.data.list
			})
			//4.给getThemeTopY赋值(对给this.themeTopYs赋值的操作进行防抖)
			this.getThemeTopY = debounce(() => {
				this.themeTopYs = []
				this.themeTopYs.push(0)
				// this.themeTopYs.push(参数的offsettop)
				this.themeTopYs.push(this.$refs.params.$el.offsetTop-44)
				this.themeTopYs.push(this.$refs.comment.$el.offsetTop-44)
				this.themeTopYs.push(this.$refs.recommend.$el.offsetTop-44)
				this.themeTopYs.push(Number.MAX_VALUE)
				// console.log(this.themeTopYs);
			},100)
		},
		mounted(){
			
		},
		destroyed(){
			this.$bus.$off('itemImageLoad',this.itemImgListener)
		},
		methods:{
			// ...mapActions(['addCart']),
			...mapActions({
				add:'addCart'
			}),
			imageLoad(){
				this.refresh()
				// console.log('+++++++');
				// this.$refs.scroll.refresh()
				this.getThemeTopY()
			},
			titleClick(index){
				console.log(index);
				this.$refs.scroll.scrollTo(0,-this.themeTopYs[index],200)
			},
			contentScroll(position){
				// console.log(position);
				//1.获取y值
				const positionY = -position.y
				//2.positionY和主题中的值做对比
				// 例：[0, 17134, 17774, 18086,100000]  自己加一个非常大的值100000
				// 例：[0, 17134, 17774, 18086,Number.MAX_VALUE]  最大值Number.MAX_VALUE
				// console.log(Number.MAX_VALUE);	
				
				//positionY在0和17134之间，index = 0
				//positionY在17134和17774之间，index = 1
				//positionY在17774和18086之间，index = 2
				//positionY超过18086，index = 3
				let length = this.themeTopYs.length
				for (let i = 0; i < length-1; i++) {
					// if(this.currentIndex !== i && ((i<length-1&&positionY>=this.themeTopYs[i]&&positionY<this.themeTopYs[i+1]) ||
					// 															(i===length-1 && positionY>=this.themeTopYs[i]))){
					// 	this.currentIndex = i;
					// 	console.log(this.currentIndex);
					// 	this.$refs.nav.currentIndex = this.currentIndex
					// }
					if(this.currentIndex !== i && (positionY >= this.themeTopYs[i] && positionY < this.themeTopYs[i+1])){
							this.currentIndex = i;
							this.$refs.nav.currentindex = this.currentIndex
					}					
				}
				//3.是否显示回到顶部
				this.isShowBackTop = (-position.y) > 1000
			},
			addToCart(){
				// console.log('-----');
				//1.获取购物车需要展示的信息
				const product = {}
				product.image = this.topImages[0];
				product.title = this.goods.title;
				product.desc = this.goods.desc;
				product.price = this.goods.realPrice;
				product.iid = this.iid;
				
				//2.将商品添加到购物车里面
				// this.$store.commit('addCart',product)
				// this.$store.dispatch('addCart',product).then(res => {
				// 	console.log(res);
				// })
				//...mapActions(['addCart'])
				this.add(product).then(res => {
					// this.isshow = true;
					// this.message = res;
					// setTimeout(() => {
					// 	this.isshow = false;
					// },1500)
					
					this.$toast.show(res,2000)
					// this.$toast.show()  //默认
				})
								
			}
		}
	}
</script>

<style scoped>
	#detail{
		position: relative;
		z-index: 9;
		background-color: #fff;
		height: 100vh;
	}
	.content{
		height: calc(100% - 44px - 49px);
	}
	.detail-nav{
		position: relative;
		z-index: 9;
		background-color: #fff;
	}
</style>
