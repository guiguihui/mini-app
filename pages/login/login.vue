<template>
	<view style="height:100vh;background: #fff;">
		<view class="img-a">
			<view class="t-b">
				您好，欢迎使用停车管理系统
			</view>
		</view>
		<view class="login-view" style="">
			<view class="t-login">
				<form class="cl">
					<view class="t-a">
						<text class="txt">帐号</text>
						<input type="number" name="id" placeholder="请输入您的帐号" maxlength="20" v-model="id" />
					</view>
					<view class="t-a">
						<text class="txt">密码</text>
						<input type="password" name="password" maxlength="50" placeholder="请输入您的密码"
							v-model="password" />
					</view>
					<button @tap="login()">登 录</button>
					<view class="reg" @tap="reg()">注 册</view>
				</form>
				<view class="t-f"><text>—————— MADE BY huihui ——————</text></view>
				<view class="t-e cl">
					<view class="t-g"></view>
					<view class="t-g"></view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				id: '', //帐号
				password: '' //密码
			};
		},
		onLoad() {},
		methods: {


			//当前登录按钮操作
			login() {
				var that = this;
				if (!that.id) {
					uni.showToast({
						title: '请输入您的帐号!',
						icon: 'none'
					});
					return;
				}
				if (!that.password) {
					uni.showToast({
						title: '请输入您的密码!',
						icon: 'none'
					});
					return;
				}
				var _this = this;
				wx.getUserProfile({
					desc: '用于完善会员资料', //这句话一定要填 不然直接报错
					success: (res) => {
						uni.setStorageSync('avatarUrl', res.userInfo.avatarUrl);
						console.log(res.userInfo.avatarUrl)
					},
					complete() {
						//等用户同意授权之后再进行登录验证
						wx.request({
							url: 'http://localhost:80/users/login2',
							data: {
								id: _this.id,
								password: _this.password
							},
							success(res) {
								if (res.data) {
									uni.showToast({
										title: '登录成功 欢迎您',
										icon: 'success'
									});
									uni.setStorage({
										key: 'user',
										data: res.data,
										success: function() {

										}
									});
									setTimeout(function() {
										wx.redirectTo({
											url: '/pages/map/map'
										})
									}, 800) //延迟时间 
								} else {
									uni.showToast({
										title: '帐号或密码错误',
										icon: 'error'
									});
								}
							},
							fail() {
								uni.showToast({
									title: '网络连接失败',
									icon: 'error'
								});
							}
						})
					}
				})

			},
			//注册按钮点击
			reg() {
				uni.showToast({
					title: '注册跳转',
					icon: 'none'
				});
				wx.redirectTo({
					url: '/pages/registe/registe'
				})
			}
		}
	};
</script>

<style>
	.txt {
		font-size: 32rpx;
		font-weight: bold;
		color: #333333;
	}

	.img-a {
		width: 100%;
		height: 450rpx;
		background-image: url(https://zhoukaiwen.com/img/loginImg/head.png);
		background-size: 100%;
	}

	.reg {
		font-size: 28rpx;
		color: #fff;
		height: 90rpx;
		line-height: 90rpx;
		border-radius: 50rpx;
		font-weight: bold;
		background: #f5f6fa;
		color: #000000;
		text-align: center;
		margin-top: 30rpx;
	}

	.login-view {
		width: 100%;
		position: relative;
		margin-top: -120rpx;
		background-color: #ffffff;
		border-radius: 8% 8% 0% 0;
	}

	.t-login {
		width: 600rpx;
		margin: 0 auto;
		font-size: 28rpx;
		padding-top: 80rpx;
	}

	.t-login button {
		font-size: 28rpx;
		background: #2796f2;
		color: #fff;
		height: 90rpx;
		line-height: 90rpx;
		border-radius: 50rpx;
		font-weight: bold;
	}

	.t-login input {
		height: 90rpx;
		line-height: 90rpx;
		margin-bottom: 50rpx;
		border-bottom: 1px solid #e9e9e9;
		font-size: 28rpx;
	}

	.t-login .t-a {
		position: relative;
	}

	.t-b {
		text-align: left;
		font-size: 42rpx;
		color: #ffffff;
		padding: 130rpx 0 0 70rpx;
		font-weight: bold;
		line-height: 70rpx;
	}

	.t-login .t-c {
		position: absolute;
		right: 22rpx;
		top: 22rpx;
		background: #5677fc;
		color: #fff;
		font-size: 24rpx;
		border-radius: 50rpx;
		height: 50rpx;
		line-height: 50rpx;
		padding: 0 25rpx;
	}

	.t-login .t-d {
		text-align: center;
		color: #999;
		margin: 80rpx 0;
	}

	.t-login .t-e {
		text-align: center;
		width: 250rpx;
		margin: 80rpx auto 0;
	}

	.t-login .t-g {
		float: left;
		width: 50%;
	}

	.t-login .t-e image {
		width: 50rpx;
		height: 50rpx;
	}

	.t-login .t-f {
		text-align: center;
		margin: 150rpx 0 0 0;
		color: #666;
	}

	.t-login .t-f text {
		margin-left: 20rpx;
		color: #aaaaaa;
		font-size: 27rpx;
	}

	.t-login .uni-input-placeholder {
		color: #aeaeae;
	}

	.cl {
		zoom: 1;
	}

	.cl:after {
		clear: both;
		display: block;
		visibility: hidden;
		height: 0;
		content: '\20';
	}
</style>
