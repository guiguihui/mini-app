<template>
	<view class="content">
		<scroll-view class="list-scroll-content" scroll-y>
			<!-- 订单列表 -->
			<view v-for="(item,index) in reserves" :key="index" class="order-item">
				<uni-card class="uni-card" :title="items[index]">

					<view class="uni-body">
						<text class="uni-title">预订编号</text>
						<text class="uni-data">{{reserves[index].reserveId}}</text>
					</view>

					<view class="uni-body">
						<text class="uni-title">停车位编号</text>
						<text class="uni-data">{{reserves[index].spaceId}}</text>
					</view>

					<view class="uni-body">
						<text class="uni-title">车牌号</text>
						<text class="uni-data">{{reserves[index].carId}}</text>
					</view>
					
					<view class="uni-body">
						<text class="uni-title">开始预定时间</text>
						<text class="uni-data">{{reserves[index].reserveStart}}</text>
					</view>
					<view class="uni-body">
						<text class="uni-title">结束预定时间</text>
						<text class="uni-data">{{reserves[index].reserveEnd}}</text>
					</view>
					
					<view class="uni-body">
						<text class="uni-title">预约费用</text>
						<text class="uni-data">{{reserves[index].reserveFee}}</text>
					</view>
					
				</uni-card>
			</view>

		</scroll-view>

	</view>
</template>

<script>
	import uCard from "@/components/uni-card/uni-card.vue"
	export default {
		data() {
			return {
				items: [],
				userinfo: {},
				reserves: {},
			}
		},

		components: {
			uCard
		},
		onLoad() {
			this.init();
		},
		methods: {
			/*用来转化时间的函数*/
			getTime: function(time) {
				var date = new Date(time),
					year = date.getFullYear(),
					month = date.getMonth() + 1,
					day = date.getDate(),
					hour = date.getHours() < 10 ? "0" + date.getHours() : date.getHours(),
					minute = date.getMinutes() < 10 ? "0" + date.getMinutes() : date.getMinutes(),
					second = date.getSeconds() < 10 ? "0" + date.getSeconds() : date.getSeconds();
				month >= 1 && month <= 9 ? (month = "0" + month) : "";
				day >= 0 && day <= 9 ? (day = "0" + day) : "";
				var timer = year + '-' + month + '-' + day + ' ' + hour + ':' + minute + ':' + second;
				return timer;
			},
			init() {
				var _this = this;
				uni.getStorage({
					key: 'user',
					success: function(res) {
						wx.request({
							url: 'http://localhost:80/reserves/car/' + res.data.carId,
							success(res) {
								if (res.data) {
									for (var i = 0; i < res.data.length; i++) {
										res.data[i].reserveStart = _this.getTime(res.data[i].reserveStart);
										res.data[i].reserveEnd = _this.getTime(res.data[i].reserveEnd);
										wx.request({
											url: 'http://localhost:80/parkings/' + res.data[i]
												.parkingId,
											success(res) {
												if (res.data) {
													_this.items.push(res.data.data
														.parkingName);
													var items2 = JSON.parse(JSON.stringify(
														_this.items))
												}
											}
										})
									}
									_this.reserves = res.data;
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

<style lang="scss">
/* 	.order-item {
		color: aqua;
	}
 */
	.uni-title {
		display: inline-block;
	}

	.uni-data {
		float: right;
	}
</style>
