<template>
	<view class="content">
		<scroll-view class="list-scroll-content" scroll-y>
			<!-- 订单列表 -->
			<view v-for="(item,index) in bills" :key="index" class="order-item">
				<!-- @click="toOrder(item)" -->
				<uni-card class="uni-card" :title="bills[index].parkingId"
					:extra="bills[index].reserveFee + bills[index].billFee">
					<view class="uni-body">
						<text class="uni-title">订单编号</text>
						<text class="uni-data">{{bills[index].billId}}</text>
					</view>
					<view class="uni-body">
						<text class="uni-title">预订编号</text>
						<text class="uni-data">{{bills[index].reserveId}}</text>
					</view>
					<view class="uni-body">
						<text class="uni-title">停车位编号</text>
						<text class="uni-data">{{bills[index].spaceId}}</text>
					</view>
					<view class="uni-body">
						<text class="uni-title">车牌号</text>
						<text class="uni-data">{{bills[index].carId}}</text>
					</view>
					<view class="uni-body">
						<text class="uni-title">开始停车时间</text>
						<text class="uni-data">{{bills[index].billStart}}</text>
					</view>
					<view class="uni-body">
						<text class="uni-title">结束停车时间</text>
						<text class="uni-data">{{bills[index].billEnd}}</text>
					</view>
					<view class="uni-body">
						<text class="uni-title">预约费用</text>
						<text class="uni-data">{{bills[index].reserveFee}}</text>
					</view>
					<view class="uni-body">
						<text class="uni-title">停车费用</text>
						<text class="uni-data">{{bills[index].billFee}}</text>
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
				//array:[],
				items: [],
				items2:[],
				userinfo: {},
				bills: {},
			}
		},

		components: {
			uCard
		},
		onLoad() {
			this.init();
		},
		methods: {
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
							url: 'http://localhost:80/bills/car/' + res.data.carId,
							success(res) {
								if (res.data) {
									for (var i = 0; i < res.data.length; i++) {
										res.data[i].billStart = _this.getTime(res.data[i].billStart);
										res.data[i].billEnd = _this.getTime(res.data[i].billEnd);
										/* wx.request({
											url: 'http://localhost:80/parkings/' + res.data[i].parkingId,
											success(res) {
												if (res.data) {
													_this.items.push(res.data.data.parkingName);
													var array = JSON.parse(JSON.stringify(_this.items))
													//console.log(_this.bills[0].parkingId)
													//console.log(array)
													//_this.parkingName = res.data.data.parkingName
													//console.log(i)
													// console.log(j)
													//_this.bills[i].parkingId = res.data.data.parkingName;
												}
											}
										}) */
										// console.log(_this.bills[0]) 
										//_this.bills[i].parkingId = _this.items.pop();
									}
									_this.bills = res.data;
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


</style>
