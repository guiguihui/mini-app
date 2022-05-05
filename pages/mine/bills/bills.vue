<template>
	<view>
		



	</view>
</template>

<script>
	export default {
		data() {
			return {
				userinfo: {},
			}
		},
		onLoad() {
			//加载
			this.init();
			console.log("load")
		},
		methods: {
			init() {
				var _this = this;
				uni.getStorage({
					key: 'user',
					success: function(res) {
						_this.userinfo = res.data;
						wx.request({
							url: 'http://localhost:80/bills/car/' + res.data.carId,
							data: {
								
							},
							success(res) {
								if (res.data) {
									console.log(res.data)
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
					fail() {
						uni.showToast({
							title: "获取账单失败",
							icon: "error"
						});
					}
				});

			},

		}
	}
</script>

<style>

</style>
