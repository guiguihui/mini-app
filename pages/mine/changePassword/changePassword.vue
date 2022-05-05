<template>
	<view class="content">
		<uni-forms :rules="rules" validate-trigger="bind" class="login_from">

			<uni-forms-item class="login_from_input" name="userPassword">
				<view class="login_from_name">原始密码</view>
				<view class="login_from_fun"><input type="text" v-model="oldPassword" password="true"
						placeholder="请输入原始密码"></view>
			</uni-forms-item>

			<uni-forms-item class="login_from_input" name="userPassword">
				<view class="login_from_name">新密码</view>
				<view class="login_from_fun"><input type="text" v-model="newPassword" password="true"
						placeholder="请输入新密码"></view>
			</uni-forms-item>

			<uni-forms-item class="login_from_input">
				<view class="login_from_name">确认密码</view>
				<view class="login_from_fun"><input type="text" v-model="newPassword2" password="true"
						placeholder="请再次输入密码"></view>
			</uni-forms-item>

			<uni-forms-item class="login_from_dl">
				<button form-type="submit" @click="formSubmit">修 改 密 码</button>
			</uni-forms-item>
		</uni-forms>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				oldPassword: '',
				newPassword: '',
				newPassword2: '',
				user: {
					userPassword: '',
					userName: '',
					carId: '',
					userGender: 0,
					userPhone: '',
					userId:0
				},
			}
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
						_this.user.userGender = res.data.userGender;
						_this.user.userPhone = res.data.userPhone;
						_this.user.carId = res.data.carId;
						_this.user.userId = res.data.userId;
						_this.user.userPassword = res.data.userPassword;
					}
				});
			},
			
			formSubmit(e) {
				var _this = this;
/* 				console.log("外边的this");
				console.log(_this); */
				
				if (this.oldPassword.length < 1) {
					uni.showToast({
						icon: 'none',
						title: '请填写原始密码'
					});
					return;
				}
				
				if(this.oldPassword != _this._data.user.userPassword){
/* 					console.log("***********************************");
					console.log("里边的this");
					console.log(this); */
					uni.showToast({
						icon: 'none',
						title: '旧密码错误'
					});
					return;
				}

				if (this.newPassword.length < 1) {
					uni.showToast({
						icon: 'none',
						title: '请填写新密码'
					});
					return;
				}
				if (this.newPassword2.length < 1) {
					uni.showToast({
						icon: 'none',
						title: '请确认新密码'
					});
					return;
				}
				if (this.newPassword != this.newPassword2) {
					uni.showToast({
						icon: 'none',
						title: '两次输入密码不一致'
					});
					return;
				}else{
					_this._data.user.userPassword = this.newPassword
				}

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
							setTimeout(function () {
							    wx.redirectTo({
							      url: '/pages/mine/mineTest'
							    })
							}, 800) //延迟时间 
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

		}
	}
</script>


<style>
	page {
		background: #fff;
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
		width: 100%;
		height: 50px;
		line-height: 50px;
		margin-top: 150upx;
	}

	.login_from_dl button {
		width: 100%;
		height: 50px;
		line-height: 50px;
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
