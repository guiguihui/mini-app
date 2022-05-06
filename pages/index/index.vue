<template>
	<view class="content">
		<image class="logo" src="/static/logo.png"></image>
		<view class="text-area">
			<text class="title">{{title}}</text>
		</view>
		<navigator class="fix-user shadow panel-center item-center" url="/pages/login/login">
		  <image src="/static/logo.png" class="icon-40"></image>
		</navigator>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Hello World'
			}
		},
		onLoad() {
			uni.getStorage({
				key: 'user',
				success: function (res) {
					//console.log(res.data);
				}
			});
		},
		methods: {

		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
	
	.icon-40{
	  width: 40rpx;
	  height: 40rpx
	}
	
	.fix-user{
	  width: 100rpx;
	  height: 100rpx;
	  border-radius: 100rpx 0 0 100rpx;
	  position: fixed;
	  top: 120rpx;
	  right: 0rpx;
	  background-color: #fff;
	}
</style>
