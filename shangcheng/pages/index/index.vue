<template>
	<view class="home">
		<swiper indicator-dots autoplay circular>
			<swiper-item v-for="item in swipers" :key="item.id">
				<image :src="item.img"></image>
			</swiper-item>
		</swiper>
		<view class="nav">
			<view class="nav-item" v-for="(item,index) in navs" :key="index" @click="navItemClick(item)">
				<view :class="item.icon"></view>
				<text>{{item.title}}</text>
			</view>
		</view>
		<view class="hot_goods">
			<view class="tit">推荐商品</view>
			<!-- <view class="goods_list">
				<view class="goods_item" v-for="item in goods" :key="item.id">
					<image :src="item.img_url"></image>
					<view class="price">
						<text>￥{{item.sell_price}}</text>
						<text>￥{{item.market_price}}</text>
					</view>
					<view class="name">
						{{item.title}}
					</view>
				</view>
			</view> -->
			<goods-list :goods="goods" @goodsItemClick="goGoodsDetail"></goods-list>
		</view>

	</view>
</template>

<script>
	import goodsList from '../../components/goods-list/goods-list.vue';
	export default {
		data() {
			return {
				swipers: [],
				goods: [],
				navs: [{
						icon: "iconfont icon-ziyuan",
						title: "黑马超市",
						path: "/pages/goods/goods"
					},
					{
						icon: "iconfont icon-guanyuwomen",
						title: "联系我们",
						path: "/pages/contact/contact"
					},
					{
						icon: "iconfont icon-tupian",
						title: "社区图片",
						path: "/pages/pics/pics"
					},
					{
						icon: "iconfont icon-shipin",
						title: "学习视频",
						path: "/pages/videos/videos"
					}
				]
			}
		},
		components: {
			"goods-list": goodsList
		},
		onLoad() {
			this.getSwipers();
			this.getHotGoods();
		},
		methods: {
			async getSwipers() {
				// uni.request({
				//  url:"http://localhost:8082/api/getlunbo",
				//  success:res=>{
				//   if(res.data.status!= 0){
				// 	  return uni.showToast({
				// 		  title:"获取数据失败"
				// 	  })
				//   }
				//   this.swipers=res.data.message;
				//  }
				// })
				var res = await this.$myRequest({
					url: "/api/getlunbo"
				})
				this.swipers = res.data.message;
			},
			async getHotGoods() {
				var res = await this.$myRequest({
					url: "/api/getgoods?pageindex=1"
				})
				this.goods = res.data.message;
				console.log(res)
			},
			//导航点击的处理函数
			navItemClick(item) {
				uni.navigateTo({
					url: item.path
				})
			},
			goGoodsDetail(id){
				uni.navigateTo({
					url:"/pages/goods-detail/goods-detail?id="+id
				})
			}
		}
	}
</script>

<style lang="scss">
	.home {
		swiper {
			width: 750rpx;
			height: 380rpx;

			image {
				width: 100%;
				height: 100%;
			}
		}

		.nav {
			display: flex;

			.nav-item {
				width: 25%;
				text-align: center;

				view {
					width: 120rpx;
					height: 120rpx;
					background-color: $shop-color;
					border-radius: 50%;
					margin: 10rpx auto;
					line-height: 120rpx;
					color: #FFFFFF;
					font-size: 50rpx;

				}

				.icon-tupian {
					font-size: 45rpx;
				}

				text {
					font-size: 30rpx;
				}

			}
		}

		.hot_goods {
			background-color: #ccc;
			overflow: hidden;
			margin-top: 2rpx;

			.tit {
				color: $shop-color;
				height: 50px;
				line-height: 50px;
				text-align: center;
				letter-spacing: 20px;
				background-color: #fff;
				margin: 7rpx 0;
				font-size: 30rpx;
			}

			.goods_list {
				padding: 0 15rpx;
				display: flex;
				flex-wrap: wrap;
				justify-content: space-between;

				.goods_item {
					background-color: #FFFFFF;
					width: 355rpx;
					margin: 10rpx 0;
					padding: 15rpx;
					box-sizing: border-box;

					image {
						width: 80%;
						height: 150rpx;
						display: block;
						margin: 0 auto;
					}

					.price {
						color: $shop-color;
						font-size: 36rpx;
						margin: 20rpx 0 0 0;

						text:nth-child(2) {
							color: #ccc;
							font-size: 28rpx;
							margin-left: 17rpx;
							text-decoration: line-through;

						}
					}

					.name {
						font-size: 28rpx;
						line-height: 50rpx;
						padding: 10rpx 0;
					}
				}
			}
		}

	}
</style>
