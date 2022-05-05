
<template>
	<view class="content">
		<uni-forms  :rules="rules" validate-trigger="bind" class="login_from">
<!-- 		<view class="login_from"> -->
			
			<uni-forms-item class="login_from_input" name="userName">
				<view class="login_from_name">姓名</view>
				<view class="login_from_fun"><input type="text"  v-model="user.userName" placeholder="请输入姓名"></view>
			</uni-forms-item>
			
			<uni-forms-item class="login_from_input" name="userPassword">
				<view class="login_from_name">密码</view>
				<view class="login_from_fun"><input type="text" v-model="user.userPassword" password="true" placeholder="请输入密码"></view>
			</uni-forms-item>
			
			<uni-forms-item class="login_from_input">
				<view class="login_from_name">确认密码</view>
				<view class="login_from_fun"><input type="text" v-model="user.userPassword2" password="true" placeholder="请再次输入密码"></view>
			</uni-forms-item>
			
			<uni-forms-item class="login_from_input" name="carId">
				<view class="login_from_name">车牌号</view>
				<view class="login_from_fun"><input type="text" v-model="user.carId" placeholder="请输入车牌号"></view>
			</uni-forms-item>

			<uni-forms-item class="login_from_input"  name="userGender" >
				<view class="login_from_name">性别</view>
				<radio-group  style=" position: absolute;left: 25%;">
					<label style="margin-right:20px;" >
						<radio value="1" @click="gender" /><text>男</text>
					</label>
					<label >
						<radio value="0"/><text>女</text>
					</label>
				</radio-group>
			</uni-forms-item>

			<uni-forms-item class="login_from_input" name="userPhone">
				<view class="login_from_name">手机号</view>
				<view class="login_from_fun"><input type="text" v-model="user.userPhone" placeholder="请输入联系方式"></view>
			</uni-forms-item>

			<uni-forms-item class="login_from_dl">
				<button form-type="submit" @click="formSubmit">注 册</button>  
			</uni-forms-item>
		</uni-forms>  
		<!-- </view> -->
	</view>
</template>

<script>
   export default {
	   data() {
	       return {
	           user: {
				    userName: '',
					userPassword: '',
					userPassword2:'',
					carId:'',
					userGender:0,
					userPhone:'',
	           },
	       }
	   },
    methods: {
		gender(){
			this.user.userGender = 1;
		},
		formSubmit(e) {
			console.log(this.user);
			if (this.user.userName.length < 1 ) {
			    uni.showToast({
			        icon: 'none',
			        title: '请输入姓名'
			    });
			    return;
			}
			if (this.user.userPassword.length < 1) {
			    uni.showToast({
			        icon: 'none',
			        title: '请填写密码'
			    });
			    return;
			}
			if (this.user.userPassword2.length < 1) {
			    uni.showToast({
			        icon: 'none',
			        title: '请确认密码'
			    });
			    return;
			}
			if (this.user.userPassword != this.user.userPassword2 ) {
			    uni.showToast({
			        icon: 'none',
			        title: '两次输入密码不一致'
			    });
			    return;
			}
			if (!/^([京津沪渝冀豫云辽黑湘皖鲁新苏浙赣鄂桂甘晋蒙陕吉闽贵粤青藏川宁琼使领A-Z]{1}[A-Z]{1}(([0-9]{5}[DF])|([DF]([A-HJ-NP-Z0-9])[0-9]{4})))|([京津沪渝冀豫云辽黑湘皖鲁新苏浙赣鄂桂甘晋蒙陕吉闽贵粤青藏川宁琼使领A-Z]{1}[A-Z]{1}[A-HJ-NP-Z0-9]{4}[A-HJ-NP-Z0-9挂学警港澳]{1})$/.test(this.user.carId)) {
			   uni.showToast({
			       icon: 'error',
			       title: '车牌号输入有误'
			   });
			   return;
			}
			if (!/^((13[0-9])|(14[5,7])|(15[0-3,5-9])|(17[0,3,5-8])|(18[0-9])|166|198|199|(147))\d{8}$/.test(this.user.userPhone)) {
			   uni.showToast({
			       icon: 'error',
			       title: '手机号输入有误'
			   });
			   return;
			}
			wx.request({
				url: 'http://localhost:80/users',
				method:'POST',
				data: this.user,
				success (res) {
					if(res.data){
						 uni.showToast({ title: '注册成功', icon: 'success' });
						/* setTimeout(function () {
						    wx.redirectTo({
						      url: '/pages/index/index'
						    })
						}, 800) //延迟时间 */
					}else{
						uni.showToast({ title: '注册失败', icon: 'error' });
					}
				},
				fail(){  
					uni.showToast({ title: '网络连接失败', icon: 'error' });
				}
			})
		},

    }
 }
</script>


<style>
	
	page{ background: #fff; }
	
	.login_img{ width: 100%; height: auto; margin-top: 90upx; }
	.login_img image{ width: 170upx; height: 170upx; display: block; margin: 0px auto; border-radius: 50%; }
	
	.login_from{ width: 100%; height: auto; box-sizing: border-box; padding: 20upx 8%; }
	
	.login_from_input{ width: 100%; height:auto; display: flex; flex-direction: row; justify-content: space-between; align-items: center; border-bottom: 1px #eee solid; padding: 40upx 0px; margin: 0px auto;  } 
	
	.login_from_name{ width: 20%; font-weight:bold; margin-left: 20px; }
	.login_from_fun{ width: 80%; display: flex; flex-direction: row; justify-content: flex-end; align-items: center;  }
	.login_from_fun input{ width: 100%; text-align: left; font-size: 14px;  }
	
	
	.login_from_dl{ width: 100%; height: 50px; line-height: 50px; margin-top: 150upx;   }
	.login_from_dl button{ width: 100%; height: 50px; line-height: 50px; background:#2796f2; color: #fff; border-radius: 50px; }
	
	.logo_xieyi{ width: 100%; height: 40px; line-height: 40px; display: flex; flex-direction: row; margin-top: 30upx; align-items: center; color: #444; font-size: 14px; justify-content: center; }
	.logo_xieyi label{ font-size: 18px; margin-right: 1%; display: block; width: 15px; height: 15px; }

	
</style>



