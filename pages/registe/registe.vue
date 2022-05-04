<template>
	<view  class="content">  
	        <form :rules="rules" @submit="formSubmit">
	
	            <view prop="userName" class="uni-form-item uni-column">
	            	<view class="title">姓名</view>
	            	<input class="uni-input" name="userName" v-model="user.userName" placeholder="请输入姓名" />
	            </view>
				
	            <view class="uni-form-item uni-column">
	            	<view class="title">密码</view>
	            	<input class="uni-input" password name="password" v-model="user.userPassword" placeholder="请设置密码" />
	            </view>
	            <view class="uni-form-item uni-column">
	            	<view class="title">确认密码</view>
	            	<input class="uni-input" password name="comfirmPwd" placeholder="请再次输入密码" />
	            </view>
				
				<view class="uni-form-item uni-column">
					<view class="title">性别</view>
					<radio-group name="radio" v-model="user.userGender">
						<label>
							<radio value="1" /><text>男</text>
						</label>
						<label>
							<radio value="0" /><text>女</text>
						</label>
					</radio-group>
				</view>
	            
	            <view class="uni-form-item uni-column">
	            	<view class="title">车牌号</view>
	            	<input class="uni-input" name="plate" v-model="user.carId" placeholder="请输入车牌号" />
	            </view>
				
	            
	            <view class="uni-form-item uni-column">
	            	<view class="title">手机号</view>
	            	<input class="uni-input" name="phone"  @blur="teltest()" v-model="user.userPhone" placeholder="请输入联系方式" />
	            </view>
				
	            <button form-type="submit">注 册</button>  
	        </form>
			
	
	    </view>  
		
</template>


		
<script>
	import plateInput from '@/components/uni-plate-input/uni-plate-input.vue'
	export default {
		data() {
			var validatePass = (rule, value, callback) => {
			    if (value === '') {
			        callback(new Error('请输入密码'));
			    } else {
			    if (this.ruleForm.confirmPwd !== '') {
			        this.$refs.ruleForm.validateField('confirmPwd');
			    }
				callback();
			}
		};
			var validatePass2 = (rule, value, callback) => {
			    if (value === '') {
			        callback(new Error('请再次输入密码'));
			    } else if (value !== this.ruleForm.newPwd) {
			        callback(new Error('两次输入密码不一致!'));
			    } else {
				callback();
			}
		};
			return {
			    user: {
					userName:this.userName,
					userPassword:this.userPassword,
					carId:this.carId,
					userGender:this.userGender,
					userPhone:this.userPhone
				},
				rules: {
					userName: [
					    { required: true, message: '请输入您的姓名', trigger: 'blur' },
					    { min: 2, max: 10, message: '长度在 2 到 10 个字符', trigger: 'blur' },
					    { trigger: 'blur' }
					],
				    password: [
				        { required: true, message: '请输入密码', trigger: 'blur' },
				        { min: 6, max: 16, message: '长度在 6 到 16 个字符', trigger: 'blur' },
				        { validator: validatePass, trigger: 'blur' }
				    ],
				    comfirmPwd:[
				        { required: true, message: '请确认密码', trigger: 'blur' },
				        { min: 6, max: 16, message: '长度在 6 到 16 个字符', trigger: 'blur' },
				        { validator: validatePass2, trigger: 'blur', required: true }
				    ],
					plate: [
				        { trigger: 'blur' }
				    ],
				}
			}
		},
		
	    methods: {
			formSubmit(e) {
				console.log(e);
				wx.request({
					url: 'http://localhost:80/users',
					method:'POST',
					data: this.user,
					success (res) {
						if(res.data){
							 uni.showToast({ title: '注册成功', icon: 'success' });
							setTimeout(function () {
							    wx.redirectTo({
							      url: '/pages/index/index'
							    })
							}, 800) //延迟时间
						}else{
							uni.showToast({ title: '注册失败', icon: 'error' });
						}
					}
				})
			},
			teltest(){
			    const reg = /^1([38]\d|5[0-35-9]|7[3678])\d{8}$/;
			    if (this.phone == '' || this.phone.length <= 10 || !reg.test(this.phone)) {
					uni.showToast({ title: 'shoujihaozhengque', icon: 'success' });
			    } else {
					uni.showToast({ title: '抱歉手机号不合法', icon: 'error' });
			    }
			},
	    }  
	}  
</script>

<style>

</style>