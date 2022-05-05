<template>
	<view class="content">
		<uni-forms :rules="rules" validate-trigger="bind" class="login_from">

			<uni-forms-item class="login_from_input" name="carId">
				<view class="login_from_name">车牌号</view>
				<view class="login_from_fun"><input type="text" v-model="newCarId" placeholder="请输入车牌号"></view>
			</uni-forms-item>

			<uni-forms-item class="login_from_dl">
				<button form-type="submit" @click="formSubmit">修 改 车 牌</button>
			</uni-forms-item>
		</uni-forms>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				newCarId:'',
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
						_this.newCarId =  res.data.carId;
					}
				});
			},
			
			formSubmit(e) {
				var _this = this;
				if (this.newCarId.length < 1) {
					uni.showToast({
						icon: 'none',
						title: '请填写新车牌号'
					});
					return;
				}
				if (!
					/^([京津沪渝冀豫云辽黑湘皖鲁新苏浙赣鄂桂甘晋蒙陕吉闽贵粤青藏川宁琼使领A-Z]{1}[A-Z]{1}(([0-9]{5}[DF])|([DF]([A-HJ-NP-Z0-9])[0-9]{4})))|([京津沪渝冀豫云辽黑湘皖鲁新苏浙赣鄂桂甘晋蒙陕吉闽贵粤青藏川宁琼使领A-Z]{1}[A-Z]{1}[A-HJ-NP-Z0-9]{4}[A-HJ-NP-Z0-9挂学警港澳]{1})$/
					.test(this.newCarId)) {
					uni.showToast({
						icon: 'error',
						title: '车牌号输入有误'
					});
					return;
				}else{
					_this.user.carId = this.newCarId
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
