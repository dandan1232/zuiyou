<template>
	<view>

		<view class="w-100" style="margin-top: 40rpx;">
			<image class="w-100" style="height: 100px;" src="../../static/login/title.png" />
		</view>

		<view class="px-4" style="padding-top: 60rpx;padding-bottom: 70rpx; ">
			<template v-if="!status">
				<view class="mb-2">
					<input type="text" v-model="loginName" placeholder="用户名" class="border-bottom p-2" />
				</view>
				<view class="mb-2 flex align-stretch">
					<input type="password" v-model="password" placeholder="密码" class="border-bottom p-2 flex-1" />
					<view class="border-bottom flex align-center justify-center font text-muted" style="width: 150rpx;"></view>
				</view>
				<view class="text-left font ml-5 text-primary" @click="changeStatus">{{status?'已有登录帐号':'立即注册'}}</view>
				
				<view class="py-2 px-3">
					<button class="bg-main text-white" style="border-radius: 50rpx; border: 0;" type="primary" :disabled="disabled"
					 :class="disabled ? 'bg-main-disabled':'bg-main'" @click="login" :loading="loading">
						{{loading?'登录中...':'登录'}}
					</button>
				</view>
			</template>
			
			<template v-else>
				<view class="mb-2 flex align-stretch">
					<input type="text" v-model="loginName" placeholder="用户名" class="border-bottom p-2" />
				</view>
				<view class="mb-2 flex align-stretch">
					<input type="password" v-model="password" placeholder="密码" class="border-bottom p-2 flex-1" />
					<view class="border-bottom flex align-center justify-center font text-muted" style="width: 150rpx;"></view>
				</view>
				<view class="text-left font ml-5 text-primary" @click="changeStatus">{{status?'已有登录帐号':'立即注册'}}</view>
				
				<view class="py-2 px-3">
					<button class="bg-main text-white" style="border-radius: 50rpx; border: 0;" type="primary" :disabled="disabled"
					 :class="disabled ? 'bg-main-disabled':'bg-main'" @click="submit" :loading="loading">
						{{loading?'注册中...':'注册'}}
					</button>
				</view>
			</template>
			
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				status: false,
				password: '',
				loginName: '',
				loading: false
			};
		},
		onLoad() {
			// uni.request({
			//             url: 'http://localhost/:8080/api/v1/user/login', 
			// 			//仅为示例，并非真实接口地址。
			//             method: 'POST',
			//             header: {
			//             'custom-header': 'userId',
			//             'custom-header': 'token',
			//                 'custom-header': 'Android', //自定义请求头信息
			//             'content-type': 'application/json'
			//             },
			          
			//           data: {
			//             "loginName": this.loginName,
			//             "passwordMd5": this.password,
			// 			// text: 'uni.request'
			//           },
			//             success: (res) => {
			//                 console.log(res.data);
			//                 console.log('token',data.token);
			//                 console.log('userId',data.userId);
			//                 this.text = 'request success';
			//             }
			//         });
			uni.request({
			            url: 'http://localhost:8080/api/v1/user/login', //仅为示例，并非真实接口地址。
			            method: 'POST',
			            header: {
			            'custom-header': 'userId',
			            'custom-header': 'token',
			                'custom-header': 'Android', //自定义请求头信息
			            'content-type': 'application/json'
			            },
			          
			          data: {
			            "loginName": this.loginName,
			            "passwordMd5": this.password
			          },
			            success: (res) => {
			                console.log(res.data);
			                this.text = 'request success';
			            }
			        });
			
		},
		computed: {
			
			disabled() {
				if (this.phone === '' || this.password === ''){
					return true;
				}
				return false;
			}
		},
		methods: {
			
			back() {
				uni.navigateBack({
					delta:1
				})
			},
			//初始化表单
			initForm() {
				this.loginName = '';
				this.password = '';
			},
			//切换登录方式
			changeStatus() {
				this.initForm();
				this.status = !this.status;
			},
			
			//表单验证
			validate() {
				//手机号正则表达式
				var mPattern = /^1[34578]\d{9}$/;
				if (!mPattern.test(this.loginName)) {
					uni.showToast({
						title: '手机号格式不正确',
						icon: 'none'
					});
					return false;
				}
				//更多验证
				return true;
			},
			login(){
			
			},
			submit() {
				}
		}
	}
</script>

<style>

</style>
