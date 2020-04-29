<template>
	<view class="news">
		<news-item :newsList="newsList" @itemClick="goDetail"></news-item>
		<!-- 	<view class="news_item" v-for="(item,index) in newsList" :key="index">
			<image :src="item.img_url"></image>
			<view class="right">
				<view class="tit">
					{{item.title}}
				</view>
				<view class="info">
					<text>发布时间：{{item.add_time}}</text>
					<text>浏览：{{item.click}}</text>
				</view>
			</view>
		</view> -->
	</view>
</template>

<script>
	import newsItem from '../../components/news-item/news-item.vue';
	export default {
		data() {
			return {
				newsList: []
			}
		},
		onLoad() {
			this.getNews()
		},
		components: {
			"news-item": newsItem,
		},
		methods: {
			async getNews() {
				const res = await this.$myRequest({
					url: "/api/getnewslist"
				})
				this.newsList = res.data.message;
				console.log(res)
			},
			goDetail(id) {
				uni.navigateTo({
					url: '/pages/news-detail/news-detail?id=' + id
				})
			}
		}
	}
</script>

<style lang="scss">
	.news {
		.news_item {
			display: flex;
			padding: 10rpx 20rpx;
			border-bottom: 1px solid $shop-color;

			image {
				width: 200rpx;
				height: 150rpx;
			}

			.right {
				margin-left: 15rpx;
				display: flex;
				flex-direction: column;
				justify-content: space-between;

				.tit {
					font-size: 30rpx;

				}

				.info {
					font-size: 24rpx;

					text:nth-child(2) {
						margin-left: 30rpx;
					}
				}
			}
		}

	}
</style>
