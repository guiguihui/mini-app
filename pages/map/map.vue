<template>
	<view class="content">

		<uni-search-bar bgColor=#F8F8F8 @confirm="search" v-model="searchValue" placeholder="搜索附近停车场">
		</uni-search-bar>


		<drag-button :isDock="true" :existTabBar="true" @btnClick="btnClick" />

		<view class="scroll">
			<scroll-view class="list-scroll-content" scroll-y>
				<!-- 停车场列表 -->
				<view v-for="(item,index) in parking" :key="index" class="order-item">
					<uni-card class="uni-card" :title="items[index]">
						<view class="uni-body">`
							<text class="uni-title">停车场名称</text>
							<text class="uni-data">{{parking[index].parkingName}}</text>
						</view>
						<view class="uni-body">
							<text class="uni-title">当前空闲停车位</text>
							<text class="uni-data">{{parking[index].parkingAvailable}}</text>
						</view>
						<view class="uni-body">
							<text class="uni-title">停车收费标准</text>
							<text class="uni-data">{{parking[index].parkingFee}}</text>
						</view>
					</uni-card>
				</view>
			</scroll-view>
		</view>

		<map :latitude="latitude" :longitude="longitude" :scale="scale" :markers="markers" show-location>
		</map>
	</view>

</template>
<script>
	import dragButton from '../../components/drag-button.vue';
	export default {
		components: {
			dragButton,
		},
		data() {
			return {
				latitude: 0,
				longitude: 0,
				scale: 16,
				markers: [],
				searchValue: '',
				parking: [],
				key: '76TBZ-S3JKS-VOJOC-6C4CR-W4NT6-LNFVE',
			}
		},
		onLoad() {
			var _this = this;
			/*获取当前位置*/
			uni.getLocation({
				type: 'gcj02',
				success: function(res) {
					_this._data.latitude = res.latitude;
					_this._data.longitude = res.longitude;
				}
			});
			this.init();
		},
		methods: {
			/*‘我的’悬浮按钮点击跳转*/
			btnClick() {
				uni.navigateTo({
					url: "/pages/mine/mineTest"
				})
			},
			/*获取停车场位置 并在地图上显示出来*/
			init() {
				var _this = this;
				wx.request({
					url: 'http://localhost:80/parkings',
					success(res) {
						if (res.data) {
							_this.parking = [];
							for (var i = 0; i < res.data.data.length; i++) {
								//将查到的停车场显示在框里
								_this.parking.push(res.data.data[i])
								//将停车场在地图上通过markers标出来
								_this.markers.push({
									id: res.data.data[i].parkingId,
									latitude: res.data.data[i].parkingLatitude,
									longitude: res.data.data[i].parkingLongitude,
									title: res.data.data[i].parkingName,
									callout: {
										content: res.data.data[i].parkingName,
										padding: 10,
										display: 'ALWAYS',
										textAlign: 'center',
									},
								});
							}
							//console.log(_this.markers)
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

			search(res) {
				var _this = this;
				wx.request({
					url: 'http://localhost:80/parkings/',
					data: {
						parkingName: this.searchValue
					},
					success(res) {
						if (res.data) {
							_this.parking = [];
							for (var i = 0; i < res.data.data.length; i++) {
								_this.parking.push(res.data.data[i])
							}
							//console.log(_this.parking)
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
		},
		onBackPress() {
			// #ifdef APP-PLUS
			plus.key.hideSoftKeybord();
			// #endif
		}

	}
</script>
<style>
	.content {
		height: 100vh;
		width: 100%;
	}

	.scroll {
		max-height: 400rpx;
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

	.uni-title {
		display: inline-block;
	}

	.uni-data {
		float: right;
	}

	map {
		height: 100vh;
		width: 100%;
	}
</style>
