<template>
	<view>
		
		reserve
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
		onShow() {
			this.init();
			console.log("show")
		},
		methods: {
			init() {
				var _this = this;
				uni.getStorage({
					key: 'user',
					success: function(res) {
						_this.userinfo = res.data;
						console.log(userinfo);
					}
				});
			},
			
		}
	}
</script>

<style>

</style>
