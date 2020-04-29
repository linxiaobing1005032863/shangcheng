<template>
	<view class="contact">
		<image src="../../static/logo.png"></image>
		<view class="info">
			<view @click="phone">联系电话：4000-9000-9000(点击拨打)</view>
			<view>校区地址:广东省广州市天气</view>
		</view>

		<map class="map" :longitude="longitude" :scale="scale" :latitude="latitude" :markers="markers"></map>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				longitude: 120.111,
				latitude: 30.890,
				scale: 13,
				markers: [{
					longitude: 120.111,
					latitude: 30.890,
					iconPath: '../../static/logo.png',
					width: 30,
					height: 30
				}]
			}
		},
		methods: {
			phone() {
				uni.makePhoneCall({
					phoneNumber: '15220037855'
				})
			}
		}
	}
</script>

<style lang="scss">
	.contact {
		image {
			width: 750rpx;
			height: 320rpx;
		}

		.info {
			padding: 10rpx 20rpx;
			font-size: 30rpx;

			view {
				line-height: 50rpx;
				border-bottom: 1px solid #eee;
			}
		}

		.map {
			width: 100%;
			height: 500px;
		}
	}
</style>
