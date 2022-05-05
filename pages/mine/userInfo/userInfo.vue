<template>
	<view class="content">
		<uni-forms :rules="rules" validate-trigger="bind" class="login_from">

			<view class="face">
				<image :src="avatarUrl"></image>
			</view>

			<uni-forms-item class="login_from_input" name="userName">
				<view class="login_from_name">姓名</view>
				<view class="login_from_fun"><input type="text" v-model="user.userName" :placeholder="user.userName">
				</view>
			</uni-forms-item>

			<uni-forms-item class="login_from_input" name="userGender">
				<view class="login_from_name">性别</view>
				<radio-group style=" position: absolute;left: 25%;">
					<label style="margin-right:20px;">
						<radio value="1" @click="gender" :checked="nan" /><text>男</text>
					</label>
					<label>
						<radio value="0" :checked="nv" /><text>女</text>
					</label>
				</radio-group>
			</uni-forms-item>

			<uni-forms-item class="login_from_input" name="userPhone">
				<view class="login_from_name">手机号</view>
				<view class="login_from_fun"><input type="text" v-model="user.userPhone" :placeholder="user.userPhone">
				</view>
			</uni-forms-item>

			<uni-forms-item class="login_from_dl">
				<button form-type="submit" @click="formSubmit">修 改</button>
			</uni-forms-item>
		</uni-forms>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				nan:0,
				nv:0,
				avatarUrl: "../../static/logo.png",
				user: {
					userName: '',
					userGender: 0,
					userPhone: '',
					userPassword:"",
					carId:"",
					userId:0,
				},
			}
		},
		onLoad() {
			//加载
			this.init();
			this.avatarUrl = uni.getStorageSync('avatarUrl');
		},
		onShow() {
			this.init();
		},
		methods: {
			init() {
				var _this = this;
				uni.getStorage({
					key: 'user',
					success: function(res) {
						_this.user.userName = res.data.userName;
						if(res.data.userGender == 1){
							_this.nan = 1;
						}else{
							_this.nv = 1;
						}
						_this.user.userPhone = res.data.userPhone;
						_this.user.userPassword = res.data.userPassword;
						_this.user.carId = res.data.carId;
						_this.user.userId = res.data.userId;
					}
				});
			},
			gender() {
				this.user.userGender = 1; //用来做单选框选中事件的
			},
			formSubmit(e) {
				console.log(this.user);
				if (this.user.userName.length < 1) {
					uni.showToast({
						icon: 'none',
						title: '请输入姓名'
					});
					return;
				}
				/* if (!/^((13[0-9])|(14[5,7])|(15[0-3,5-9])|(17[0,3,5-8])|(18[0-9])|166|198|199|(147))\d{8}$/.test(this.user.userPhone)) {
					uni.showToast({
						icon: 'error',
						title: '手机号输入有误',
						
					});
					console.log(this.user.userPhone)
					return;
				} */
				wx.request({
					url: 'http://localhost:80/users',
					method: 'POST',
					data: this.user,
					success(res) {
						if (res.data) {
							uni.showToast({
								title: '修改成功',
								icon: 'success'
							});
							/* setTimeout(function () {
							    wx.redirectTo({
							      url: '/pages/index/index'
							    })
							}, 800) //延迟时间 */
						} else {
							uni.showToast({
								title: '修改失败',
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
			},
			jumpToLogin() {
				uni.showToast({
					title: '登录跳转',
					icon: 'none'
				});
				wx.redirectTo({
					url: '/pages/login/login'
				})
			}

		}
	}
</script>


<style lang="scss">
	page {
		background: #fff;
	}

	.face {
		flex-shrink: 0;
		margin:0 auto;
		width: 35vw;
		height: 35vw;
	
		image {
			width: 100%;
			height: 100%;
			border-radius: 100%
		}
	}

	.login_img {
		width: 100%;
		height: auto;
		margin-top: 90upx;
	}

	.login_img image {
		width: 170upx;
		height: 170upx;
		display: block;
		margin: 0px auto;
		border-radius: 50%;
	}

	.login_from {
		width: 100%;
		height: auto;
		box-sizing: border-box;
		padding: 20upx 8%;
	}

	.login_from_input {
		width: 100%;
		height: auto;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		align-items: center;
		border-bottom: 1px #eee solid;
		padding: 40upx 0px;
		margin: 0px auto;
	}

	.login_from_name {
		width: 20%;
		font-weight: bold;
		margin-left: 20px;
	}

	.login_from_fun {
		width: 80%;
		display: flex;
		flex-direction: row;
		justify-content: flex-end;
		align-items: center;
	}

	.login_from_fun input {
		width: 100%;
		text-align: left;
		font-size: 14px;
	}


	.login_from_dl {
		width: 80%;
		height: 50px;
		line-height: 50px;
		margin-top: 200upx;
	}

	.login_from_dl button {
		width: 80%;
		height: 50px;
		line-height: 50px;
		margin-top: 100upx;
		background: #2796f2;
		color: #fff;
		border-radius: 50px;
	}

	.logo_xieyi {
		width: 100%;
		height: 40px;
		line-height: 40px;
		display: flex;
		flex-direction: row;
		margin-top: 30upx;
		align-items: center;
		color: #444;
		font-size: 14px;
		justify-content: center;
	}

	.logo_xieyi label {
		font-size: 18px;
		margin-right: 1%;
		display: block;
		width: 15px;
		height: 15px;
	}
</style>
