<template>
	<view class="content">
		<view class="parkingInfomation">
			<view class="name">
				{{parkingName}}
			</view>
			<view class="address">
				{{parkingAddress}}
			</view>
			<view class="availablle">
				{{parkingAvailable}}
			</view>
			<view class="sapce">
				/{{parkingSpace}}
			</view>
			<view class="fee">
				{{parkingFee}}元/每小时
			</view>
		</view>



		<view class="scroll">
			<scroll-view class="list-scroll-content" scroll-y>
				<!-- 停车位列表 -->
				<view v-for="(item,index) in spaces" :key="index" class="order-item" @click="itemCilck(item,index)">
					<card class="uni-card" :title="items[index]">
						<view class="uni-body">
							<text class="uni-title">停车位ID:</text>
							<text class="uni-data">{{spaces[index].spaceId}}</text>
							<button class="btn" type="primary" v-if="spaces[index].spaceState=='空闲'">预约</button>
							<button class="btn" type="default" disabled v-else>预约</button><!--  -->

						</view>
					</card>
				</view>
			</scroll-view>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				parkingId: '',
				parkingName: '',
				parkingAddress: '',
				parkingSpace: 0,
				parkingAvailable: 0,
				parkingFee: 0,
				spaces: [],
				user: {},
			}
		},

		onLoad: function(options) {
			this.init();
			var _this = this;
			_this.parkingId = options.parkingId;
			_this.parkingName = options.parkingName;
			_this.parkingAddress = options.parkingAddress;
			_this.parkingSpace = options.parkingSpace;
			_this.parkingAvailable = options.parkingAvailable;
			_this.parkingFee = options.parkingFee;

		},
		methods: {
			init() {
				var myDate = new Date();
				console.log(myDate.toJSON())
				/*console.log(myDate.getUTCDate());

				console.log(myDate.toISOString(YYYY-MM-DD)); */
				var _this = this;
				uni.getStorage({
					key: 'user',
					success: function(res) {
						_this.user = res.data
					}
				});
				wx.request({
					url: 'http://localhost:80/spaces/parkingId',
					data: {
						parkingId: _this.$mp.query.parkingId
					},
					success(res) {
						if (res.data) {
							_this.spaces = [];
							for (var i = 0; i < res.data.length; i++) {
								//将查到的停车场显示在框里
								_this.spaces.push(res.data[i])
							}
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
			/*点击预约*/
			itemCilck(res) {
				var _this = this;
				if (res.spaceState == '空闲') {
					var space = {
						carId: _this.user.carId,
						spaceId: res.spaceId,
						parkingId: res.parkingId,
						spaceState: "占用",
						isDeleted: 0,
					};
					// if(!uni.getStorageSync('isReserve')){
					if (true) {
						wx.request({
							url: 'http://localhost:80/spaces',
							method: 'POST',
							data: space,
							success(res) {
								/*将已经预约的信息保存在缓存中*/
								

								/*创建预约单信息*/
								var myDate = new Date();
								var reserve = {
									reserveId: space.spaceId + myDate.toJSON() + _this.user.userId,
									parkingId: space.parkingId,
									spaceId: space.spaceId,
									carId: _this.user.carId,
									reserveStart: myDate.toJSON(),
								}

								wx.request({
									url: 'http://localhost:80/reserves',
									method: 'POST',
									data: reserve,
									success(res) {
										if (res.data) {
											uni.setStorageSync('isReserve', true);
											uni.showToast({
												title: '预约成功',
												icon: 'success'
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
								/*刷新页面*/
								_this.init();
							},
							fail() {
								uni.showToast({
									title: '网络连接失败',
									icon: 'error'
								});
							}
						})
					} else {
						uni.showToast({
							title: '您已进行预约',
							icon: 'error'
						});
					}
				} else {
					uni.showToast({
						title: '该车已被占用',
						icon: 'error'
					});
				}
			}
		}
	}
</script>

<style>
	.content {
		height: 100vh;
		width: 100%;
	}

	.parkingInfomation {
		margin-left: 10px;
	}


	.name {
		margin-top: 5%;
		font-size: 50rpx;
		padding: 5px 0 5px 0 ;
	}

	.address {
		font-size: 30rpx;
		padding: 5px 0 5px 0 ;
	}

	.availablle {
		font-size: 35rpx;
		display: inline-block;
		padding: 5px 0 5px 0 ;
	}

	.sapce {
		font-size: 25rpx;
		display: inline-block;
		padding: 5px 0 5px 0 ;
	}

	.fee {
		font-size: 35rpx;
		padding: 5px 0 5px 0 ;
	}



	.scroll {
		max-height: 75%;
		overflow: auto;

		width: 90%;
		margin-left: 5%;
		border-width: 0px;
		border-radius: 20rpx;
		box-shadow: 0 0 20rpx rgba(86, 119, 252, 0.2);

		bottom: 0;
		z-index: 99;
		position: fixed;

	}

	.uni-body {
		height: 40px;
		box-shadow: 0 0 20rpx rgba(86, 119, 252, 0.2);
	}

	.uni-title {
		display: inline-block;
	}

	.uni-data {}

	.btn {
		display: inline-block;
		float: right;
		line-height: 30px; //让文字垂直居中要使之与父组件高度一致
		margin-top: 5px;
		height: 30px;
	}
</style>
