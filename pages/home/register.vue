<template>
	<view class="content">
		<input type="text" v-model="cname" placeholder="请输入姓名"></input>
		<input type="text" v-model="username" placeholder="请输入用户名"></input>
		<input type="password" v-model="password" placeholder="请输入密码"></input>
		<input type="text" v-model="telno" placeholder="请输入电话"></input>
		<input type="text" v-model="email" placeholder="请输入邮箱"></input>
		<radio-group @change="changeSex">
			<label>
				<radio value="男" checked="true" />男</label>
			<label>
				<radio value="女" />女</label>
		</radio-group>
		<picker @change="bindPickerChange" :value="index" :range="array" range-key="compname">
			<view class="uni-input">{{array[index].compname}}</view>
		</picker>
		<button type="primary" @click="register">注册</button>
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
				cname: "",
				username: "",
				password: "",
				telno: "",
				email: "",
				sex: "男",
				compid: "",
				array: [{compname:''}],
				index: 0
			}
		},
		onLoad() {
			uni.request({
				url: this.url + 'baseCompany/search',
				success: (res) => {
					this.array = res.data;
				},
				fail: () => {
					uni.showToast({
						icon: 'none',
						title: '网络异常,请稍后重试',
						duration: 1000
					});
				}
			})
		},
		methods: {
			bindPickerChange(e) {
				this.index = e.target.value;
			},
			changeSex(obj) {
				this.sex = obj.detail.value;
			},
			register() {
				if (this.cname.length <= 0) {
					uni.showToast({
						icon: 'none',
						title: '请输入姓名',
						duration: 1000
					});
					return;
				}
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
				if (this.telno.length <= 0) {
					uni.showToast({
						icon: 'none',
						title: '请输入电话',
						duration: 1000
					});
					return;
				}
				if (this.email.length <= 0) {
					uni.showToast({
						icon: 'none',
						title: '请输入邮箱',
						duration: 1000
					});
					return;
				}
				//网络请求
				uni.request({
					url: this.url + 'baseUser/insert',
					data: {
						cname: this.cname,
						username: this.username,
						password: this.password,
						telno: this.telno,
						email: this.email,
						sex: this.sex,
						compid: this.array[this.index].compid
					},
					success: (res) => {
						uni.showToast({
							icon: 'success',
							title: '注册成功',
							duration: 1000
						});
						uni.redirectTo({
							url: 'login'
						});
					},
					fail: () => {
						uni.showToast({
							icon: 'none',
							title: '网络异常,请稍后重试',
							duration: 1000
						});
					}
				})
			}
		}
	}
</script>

<style>
	@import url("../../static/css/login.css");
</style>
