<template>
	<map style="width: {{mapWidth }}rpx; height: {{mapHeight}}rpx;" markers="{{markers}}"
		include-points="{{markers}}" />
</template>
<script>
	import wepy from "@wepy/core"
	export default {
		data() {
			mapWidth: '',
			mapHeight: '',

			},

			onLoad() {
				var sy = wx.getSystemInfoSync(),
					mapWidth = sy.windowWidth * 2,
					mapHeight = sy.windowHeight * 2;
				this.mapWidth = mapWidth,
					this.mapHeight = mapHeight
			}
		})
</script>
