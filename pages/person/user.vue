<template>
	<view>
		<uni-list>
			<uni-list-item title="头像" note="点击上传头像" show-extra-icon="true" :extra-icon="{color:'#A8A8A8',size:'40',type:'contact'}"></uni-list-item>
			<uni-list-item @click="fn1" title="用户名" :note="username"></uni-list-item>
			<uni-list-item @click="fn2" title="姓名" :note="cname"></uni-list-item>
			<picker :value="index" :range="sexArr" range-key="sexType" @change="changeSex">
				<uni-list-item title="性别" :note="sexArr[index].sexType"></uni-list-item>
			</picker>
			<uni-list-item @click="fn4" title="手机号码" :note="telno"></uni-list-item>
			<uni-list-item @click="fn5" title="邮箱" :note="email"></uni-list-item>
			<uni-list-item title="个性签名" note="欣赏为主，好看为辅" :show-arrow='false'></uni-list-item>
		</uni-list>
	</view>
</template>

<script>
	import uniIcon from "@dcloudio/uni-ui/lib/uni-icon/uni-icon.vue"
	import uniList from '@dcloudio/uni-ui/lib/uni-list/uni-list.vue'
	import uniListItem from '@dcloudio/uni-ui/lib/uni-list-item/uni-list-item.vue'
	export default {
		components: {
			uniIcon,
			uniList,
			uniListItem
		},
		data() {
			return {
				userid: '',
				username: '',
				cname: '',
				sex: '',
				telno: '',
				email: '',
				index: 0,
				sexArr: [{
					sexType: '男'
				}, {
					sexType: '女'
				}]
			}
		},
		onLoad() {
			uni.getStorage({
				key: 'userid',
				success: (res) => {
					this.userid = res.data;
					//后台获取需要的数据并给页面展示的数据赋值
					uni.request({
						url: this.url + 'baseUser/getUserInfo',
						data: {
							userid: this.userid
						},
						success: (res) => {
							this.username = res.data.username;
							this.cname = res.data.cname;
							this.sex = res.data.sex;
							for (var i = 0; i < this.sexArr.length; i++) {
								if (this.sexArr[i].sexType == this.sex) {
									this.index = i;
									break;
								}
							}
							this.telno = res.data.telno;
							this.email = res.data.email;
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
			})
		},
		methods: {
			changeSex(e) {
				this.index = e.target.value;
				uni.request({
					url: this.url + 'baseUser/updateSelective',
					data: {
						userid: this.userid,
						sex: this.sexArr[this.index].sexType
					},
					success: (res) => {
						uni.showToast({
							icon: 'none',
							title: '更改成功',
							duration: 1000
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
			},
			fn1() {
				uni.navigateTo({
					url: 'update?col=username'
				});
			},
			fn2() {
				uni.navigateTo({
					url: 'update?col=cname'
				});
			},
			fn4() {
				uni.navigateTo({
					url: 'update?col=telno'
				});
			},
			fn5() {
				uni.navigateTo({
					url: 'update?col=email'
				});
			}
		}
	}
</script>

<style>

</style>
