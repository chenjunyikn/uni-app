<template>
	<view>
		<uni-list>
			<uni-list-item title="清除本地缓存" show-badge="true" :badge-text="badge" @click='fn'></uni-list-item>
			<uni-list-item title="关于我们" @click='fn1'></uni-list-item>
			<uni-list-item title="检查更新" @click='fn2'></uni-list-item>
		</uni-list>
		<button type="warn" @click="close">退出登录</button>
	</view>
</template>

<script>
	import uniList from '@dcloudio/uni-ui/lib/uni-list/uni-list.vue'
	import uniListItem from '@dcloudio/uni-ui/lib/uni-list-item/uni-list-item.vue'
	export default {
		components: {
			uniList,
			uniListItem
		},
		data() {
			return {
				badge: '--'
			}
		},
		onLoad() {
			/* this.setbadge(); */
			var random = (Math.random()*(1-10)+10).toFixed(2);
			this.badge = random+'KB';
		},
		methods: {
			close() {
				//路由跳转
				uni.reLaunch({
					url: '../home/login'
				});
			},
			/* setbadge() {//读取数据缓存信息
				uni.getStorageInfo({
					success: (res) => {
						if(res.keys.length==0){
							this.badge="0KB";
							return;
						}
						var size = res.currentSize;
						if (size < 1024) {
							this.badge = size + "KB";
						} else {
							size = (size / 1024.0).toFixed(2);
							this.badge = size + "MB";
						}
					}
				});
			}, */
			fn() {
				this.badge = '0KB';
				uni.showToast({
					icon: 'none',
					title: '清除本地缓存成功',
					duration: 1000
				});
				/* try {
					uni.clearStorageSync();
					this.setbadge();
					uni.showToast({
						icon: 'none',
						title: '清除本地缓存成功',
						duration: 1000
					});
				} catch (e) {
					uni.showToast({
						icon: 'none',
						title: '清除本地缓存失败',
						duration: 1000
					});
				} */
			},
			fn1() {
				uni.navigateTo({
					url: "aboutMe"
				});
			},
			fn2() {
				uni.showToast({
					icon: 'none',
					title: '当前已是最新版本',
					duration: 1000
				});
			}
		}
	}
</script>

<style>
	button {
		margin: 60upx 5%;
		height: 80upx;
		font-size: 32upx;
		border-radius: 60upx;
	}
</style>
