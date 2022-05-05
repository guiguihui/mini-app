<template>
	<view>
		<view class="header" v-bind:class="{'status':isH5Plus}">
			<view class="userinfo" v-if="login">
				<view class="face">
					<image :src="avatarUrl"></image>
				</view>
				<view class="info">
					<view class="username">{{userinfo.userName}}</view>
					<view class="integral">手机号码:{{userinfo.userPhone}}</view>
				</view>
			</view>

			<view class="userinfo" v-else>
				<view class="face">
					<image src="../../static/logo.png"></image>
				</view>
				<view class="info">
					<navigator url="/pages/login/login">
						<view class="username" style="padding-top: 8px;padding-left: 8px" url>点击登录</view>
					</navigator>
				</view>
			</view>
		</view>

		<view class="orders">
			<view class="box">
				<view class="label" v-for="(row,index) in orderTypeLise" :key="row.name" hover-class="hover"
					@tap="toOrderType(index)">
					<view class="icon">
						<view class="badge" v-if="row.badge>0">{{row.badge}}</view>
						<image :src="'../../static/'+row.icon"></image>
					</view>
					{{row.name}}
				</view>
			</view>
		</view>

		<view class="list" v-for="(list,list_i) in severList" :key="list_i">
			<view class="li" v-for="(li,li_i) in list" @tap="toPage(list_i,li_i)"
				v-bind:class="{'noborder':li_i==list.length-1}" hover-class="hover" :key="li.name">
				<view class="icon">
					<image :src="'../../static/'+li.icon"></image>
				</view>
				<view class="text">{{li.name}}</view>
				<image :src="'../../static/'+to.icon"></image>
			</view>
		</view>

	</view>
</template>



<script>
	export default {

		data() {
			return {
				isH5Plus: true,
				login: false,
				userinfo: {},
				avatarUrl: "../../static/logo.png",
				orderTypeLise: [{
						name: '我的订单',
						icon: 'dingdan.png',
						badge: '',
						index: "0"
					},
					{
						name: '预定',
						icon: 'yuding.png',
						badge: '',
						index: "1"
					}
				],
				severList: [
					[{
							name: '个人信息',
							icon: 'geren.png',
							url: "/pages/mine/userInfo/userInfo"
						},
						{
							name: '我的车辆',
							icon: 'cheliang.png',
							url: "/pages/mine/changePlate/changePlate"
						},
					],
					[{
							name: '修改密码',
							icon: 'mima.png',
							url: "/pages/mine/changePassword/changePassword"
						},
						{
							name: '关于',
							icon: 'guanyu.png',
							url: "/pages/mine/about/about"
						}
					]
				],
			};
		},
		onLoad() {
			//加载
			this.init();
			this.avatarUrl = uni.getStorageSync('avatarUrl');
		},
		onShow() {
			if (!this.login) {
				this.init();
			}
		},
		methods: {
			init() {
				var _this = this;
				uni.getStorage({
					key: 'user',
					success: function(res) {
						_this.login = true;
						_this.userinfo = res.data;
					}
				});
			},
			//用户点击订单类型
			toOrderType(index) {
				uni.setStorageSync('order_index', this.orderTypeLise[index].index);
				console.log(uni.getStorageSync('order_index'));
				if(uni.getStorageSync('order_index')  == 0){
					uni.navigateTo({
						url: "/pages/mine/bills/bills"
					})
				}else{
					uni.navigateTo({
						url: "/pages/mine/registe/registe"
					})
				}
				
			},
			//用户点击列表项
			toPage(list_i, li_i) {
				// uni.showToast({title: this.severList[list_i][li_i].name});
				if (this.severList[list_i][li_i].url != null) {
					if (this.severList[list_i][li_i].name == "全部订单") {
						uni.setStorageSync('order_index', this.severList[list_i][li_i].index);
					}
					uni.navigateTo({
						url: this.severList[list_i][li_i].url
					})
				} else {
					uni.showToast({
						title: "该功能暂未开放"
					});
				}
			}
		}
	}
</script>

<style lang="scss">
	page {
		background-color: #fff
	}

	.header {
		&.status {
			padding-top: var(--status-bar-height);
		}

		background-color:#5677fc;
		width:92%;
		height:30vw;
		padding:0 4%;
		display:flex;
		align-items:center;

		.userinfo {
			width: 90%;
			display: flex;

			.face {
				flex-shrink: 0;
				width: 15vw;
				height: 15vw;

				image {
					width: 100%;
					height: 100%;
					border-radius: 100%
				}
			}

			.info {
				display: flex;
				flex-flow: wrap;
				padding-left: 30upx;

				.username {
					width: 100%;
					color: #fff;
					font-size: 40upx
				}

				.integral {
					display: flex;
					align-items: center;
					padding: 0 20upx;
					height: 40upx;
					color: #fff;
					background-color: rgba(0, 0, 0, 0.1);
					border-radius: 20upx;
					font-size: 24upx
				}
			}
		}

		.setting {
			flex-shrink: 0;
			width: 6vw;
			height: 6vw;

			image {
				width: 100%;
				height: 100%
			}
		}
	}

	.hover {
		background-color: #eee
	}

	.orders {
		background-color: #5677fc;
		width: 92%;
		height: 11vw;
		padding: 0 4%;
		margin-bottom: calc(11vw + 40upx);
		display: flex;
		align-items: flex-start;
		border-radius: 0 0 100% 100%;
		margin-top: -1upx;

		.box {
			width: 98%;
			padding: 0 1%;
			height: 22vw;
			background-color: #fefefe;
			border-radius: 24upx;
			box-shadow: 0 0 20upx rgba(0, 0, 0, 0.15);
			margin-bottom: 40upx;
			display: flex;
			align-items: center;
			justify-content: center;

			.label {
				display: flex;
				align-items: center;
				justify-content: center;
				flex-flow: wrap;
				width: 100%;
				height: 16vw;
				color: #666666;
				font-size: 26upx;

				.icon {
					position: relative;
					width: 7vw;
					height: 7vw;
					margin: 0 1vw;

					.badge {
						position: absolute;
						width: 4vw;
						height: 4vw;
						background-color: #5677fc;
						top: -1vw;
						right: -1vw;
						border-radius: 100%;
						font-size: 20upx;
						color: #fff;
						display: flex;
						align-items: center;
						justify-content: center;
						z-index: 10;
					}

					image {
						width: 7vw;
						height: 7vw;
						z-index: 9;
					}
				}
			}
		}
	}

	.list {
		width: 100%;
		border-bottom: solid 26upx #f1f1f1;

		.li {
			width: 92%;
			height: 100upx;
			padding: 0 4%;
			border-bottom: solid 1upx #e7e7e7;
			display: flex;
			align-items: center;

			&.noborder {
				border-bottom: 0
			}

			.icon {
				flex-shrink: 0;
				width: 50upx;
				height: 50upx;

				image {
					width: 50upx;
					height: 50upx
				}
			}

			.text {
				padding-left: 20upx;
				width: 100%;
				color: #666
			}

			.to {
				flex-shrink: 0;
				width: 40upx;
				height: 40upx
			}
		}
	}
</style>
