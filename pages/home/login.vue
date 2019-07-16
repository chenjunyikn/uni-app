<template>
	<view class="content">
		<input type="text" v-model="username" placeholder="请输入用户名"></input>
		<view class="clear"><uni-icon type="clear" color='#737373' size="20" @click='username=password=""'></uni-icon></view>
		<view>
			<input type="password" v-if="flag==true" v-model="password" placeholder="请输入密码"></input>
			<input type="text" v-if="flag==false" v-model="password" placeholder="请输入密码"></input>
			<view class="uni-icon"><uni-icon type="eye" color='#737373' size="20" @click='flagEye'></uni-icon></view>
		</view>
		<button type="primary" @click="login">登录</button>
	</view>
</template>

<script>
	import uniIcon from "@dcloudio/uni-ui/lib/uni-icon/uni-icon.vue"
	export default {
		components: {
			uniIcon
		},
		data() {
			return {
				username: "lhx",
				password: "123456",
				flag: true
			}
		},
		methods: {
			//控制密码显示和隐藏
			flagEye() {
				this.flag = !this.flag;
			},
			login() {
				//登录判断是否为空并提示用户不能为空
				if (this.username.length <= 0) {
					uni.showToast({
						icon: 'none',
						title: '请输入用户名',
						duration: 1000
					});
					return;
				}
				if (this.password.length <= 0) {
					uni.showToast({
						icon: 'none',
						title: '请输入密码',
						duration: 1000
					});
					return;
				}
				//网络请求后台
				uni.request({
					//请求地址
					url: this.url + 'baseUser/login',
					//请求数据
					data: {
						username: this.username,
						password: this.password
					},
					success: (res) => { //交互成功箭头回调函数
						//判断用户名密码是否和数据库一致
						if ((this.username != res.data.username) && (this.password != res.data.password)) {
							//不一致提示用户信息不正确
							uni.showToast({
								icon: 'none',
								title: '用户名或密码不正确',
								duration: 1000
							});
							return;
						} else { //登录成功状态
							//跳转tabBar界面
							uni.switchTab({
								url: 'index'
							});
							//提示登录成功信息
							uni.showToast({
								icon: 'success',
								title: '登录成功',
								duration: 1000
							});
							//将compid缓存到本机用于后期需要
							uni.setStorage({
								key: 'compid',
								data: res.data.compid
							});
							//将userid缓存到本机用于后期需要
							uni.setStorage({
								key: 'userid',
								data: res.data.userid
							});
						}
					},
					fail: () => { //网络连接失败后提示
						uni.showToast({
							icon: 'none',
							title: '网络异常,请稍后重试',
							duration: 1000
						});
					}
				})
			}
		},
		//监听原生标题栏注册按钮点击事件并作路由跳转
		onNavigationBarButtonTap() {
			uni.navigateTo({
				url: 'register'
			});
		}
	}
</script>

<style>
	/* 引入css外部样式表 */
	@import url("../../static/css/login.css");
</style>
