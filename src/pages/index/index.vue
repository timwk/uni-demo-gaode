<template>
	<view class="content">
		<button @tap="goMap" class="btn">跳转高德地图</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Index'
			}
		},
		onLoad() {

		},
		methods: {
			goMap() {
				uni.navigateTo({
					url: '/pages/map/index'
				})
			}
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		padding-top: 80rpx;
	}

	.btn {
		font-size: 40rpx;
		line-height: 100rpx;
		color: #fff;
		background: #0098f0;
	}
</style>
