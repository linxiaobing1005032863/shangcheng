<template>
	<view class="pics">
		<scroll-view class="left" scroll-y>
			<view @click=leftClickHandle(index,item.id) :class="active==index?'active':''" v-for="(item,index) in cates" :key="item.id">{{item.title}}</view>
		</scroll-view>
		<scroll-view class="right" scroll-y>
			<view class="item" v-for="(item,index) in secondData" :key="index">
				<image :src="item.img_url" @click="priviewImg(item.img_url)"></image>
				<text>{{item.title}}</text>
			</view>
			<text v-if="secondData.length==0">暂无数据</text>
		</scroll-view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				cates: [],
				active: 0,
				secondData: []
			}
		},
		onLoad() {
			this.getPicsCate()
		},
		methods: {
			async getPicsCate() {
				const res = await this.$myRequest({
					url: "/api/getimgcategory"
				})
				this.cates = res.data.message;
				console.log(res)
				this.leftClickHandle(0, this.cates[0].id)
			},
			async leftClickHandle(index, id) {
				this.active = index;
				console.log(id)
				const res = await this.$myRequest({
					url: "/api/getimages/" + id
				})
				this.secondData = res.data.message;
				console.log(res)
			},
			priviewImg(current) {
				const urls = this.secondData.map((item) => {
					return item.img_url;
				})
				console.log(urls)
				uni.previewImage({
					urls: urls,
					current: current
				})
			}
		}
	}
</script>

<style lang="scss">
	page {
		height: 100%;

		.pics {
			height: 100%;
			display: flex;

			.left {
				width: 200rpx;
				height: 100%;
				border-right: 1px solid #eee;

				view {
					height: 60px;
					line-height: 60px;
					color: #999;
					font-size: 30rpx;
					text-align: center;
					border-top: 1px solid #eee;

				}

				.active {
					background-color: $shop-color;
					color: #FFFFFF
				}
			}

			.right {
				width: 520rpx;
				height: 100%;
				margin: 10rpx auto;

				.item {
					image {
						width: 520rpx;
						height: 520rpx;
						border-radius: 5px;

					}

					text {
						font-size: 30rpx;
						line-height: 60rpx;
					}
				}
			}
		}
	}
</style>
