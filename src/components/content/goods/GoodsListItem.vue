<!-- <template>
  <div class="goods" @click="goToDetail">
    <img v-lazy="getImg" :key="getImg" alt="">
    <div class="goods-info">
      <p>{{goods.title}}</p>
      <span class="price">¥{{goods.price}}</span>
      <span class="collect">{{goods.cfav}}</span>
    </div>
  </div>
</template>

<script>
	export default {
		name: "GoodsListItem",
    props: {
		  goods: {
		    type: Object,
        default: {}
      }
    },
    mounted: function () {
      // console.log(this.goods);
    },
    methods: {
      goToDetail: function () {
        // 1.获取iid
        let iid = this.goods.iid;

        // 2.跳转到详情页面
        this.$router.push({path: '/detail', query: {iid}})
      }
    },
    computed: {
      getImg() {
        return this.goods.img || this.goods.image || this.goods.show.img
      }
    }
	}
</script>

<style scoped>
  .goods {
    padding-bottom: 40px;
    position: relative;
  }
  .goods img {
    width: 100%;
  }

  .goods-info {
    font-size: 12px;
    position: absolute;
    bottom: 5px;
    left: 0;
    right: 0;
    overflow: hidden;
    text-align: center;
  }

  .goods-info p {
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    margin-bottom: 3px;
  }

  .goods-info .price {
    color: var(--color-high-text);
    margin-right: 20px;
  }

  .goods-info .collect {
    position: relative;
  }

  .goods-info .collect::before {
    content: '';
    position: absolute;
    left: -15px;
    top: 0;
    width: 14px;
    height: 14px;
    background: url("~assets/img/common/collect.svg") 0 0/14px 14px;
  }
</style> -->



<template>
	<div class="goods-item" @click="itemClick">
	  <!-- <img :src="showImage" alt="" @load="imgload"> //注释 -->
		<img v-lazy="showImage" :key="showImage" alt="" @load="imgload">
		<div class="goods-info">
			<p>{{goodsItem.title}}</p>
			<span class="price">{{goodsItem.price}}</span>
			<span class="collect">{{goodsItem.cfav}}</span>
		</div>
	</div>
</template>

<script>
	export default{
		name:"GoodsListItem",
		props:{
			goodsItem:{
				type:Object,
				default(){
					return {}
				}
			}
		},
		computed:{
			showImage(){
				return this.goodsItem.img || this.goodsItem.image || this.goodsItem.show.img
			}
		},
		methods:{
			imgload(){
				this.$bus.$emit('itemImageLoad')
			},
			itemClick(){
				this.$router.push('/detail/' + this.goodsItem.iid)
				// this.$router.push({
				// 	path:'/detail',
				// 	query:{
				// 		iid:this.goodsItem.iid
				// 	}
				// })
			}
		}
	}
</script>

<style scoped>
	.goods-item{
		padding-bottom: 40px;
		position: relative;
		width: 48%;
	}
	
	.goods-item img{
		width: 100%;
		border-radius: 5px;
	}
	
	.goods-info{
		font-size: 12px;
		position: absolute;
		bottom: 5px;
		left: 0;
		right: 0;
		overflow: hidden;
		text-align: center;
	}
	
	.goods-info p{
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
		margin-bottom: 3px;
	}
	
	.goods-info .price{
		color: var(--color-high-text);
		margin-right: 20px;
	}
	
	.goods-info .collect{
		position: relative;;
	}
	
	.goods-info .collect::before{
		content: '';
		position: absolute;
		left: -15px;
		top: -1px;
		width: 14px;
		height: 14px;
		background: url("~assets/img/common/collect.svg") 0 0/14px 14px;
	}
</style>
