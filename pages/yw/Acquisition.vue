<template>
	<view>
		<view class="top">
			<view class="zhuyi">注意：此业务用于企业不同数据库间数据的一个同步作用，点击右侧按钮就可以触发...</view>
			<view class="btn"><button type="warn" class="AcqBtn" @click="synchro">数据同步</button></view>
		</view>
		<!-- 同步信息 -->
		<tableAcquisition></tableAcquisition>
	</view>
</template>

<script>
	import tableAcquisition from '../table/tableAcquisition.vue'
	export default {
		components:{tableAcquisition},
		data() {
			return {
				userid:'',
				compid:''
			}
		},
		methods:{
			synchro(){
				//在本地缓存中取值compid并给页面compid变量赋值
				//getStorage和request请求是异步交互，同时执行，所以将request嵌套getStorage
				uni.getStorage({
					key: 'userid',
					success: (res) => {
						this.userid = res.data;
					}
				});
				uni.getStorage({
					key: 'compid',
					success: (res) => {
						this.compid = res.data;
						//后台获取需要的数据并给页面展示的数据赋值
						uni.request({
							url: this.url + 'synchro/initCurrentDay',
							data: {
								userid: this.userid,
								compid: this.compid
							},
							success: (res) => {
								uni.showToast({
									icon: 'success',
									title: '同步成功',
									duration: 1000
								});
								uni.reLaunch({
									url: '../home/yw'
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
				})
			}
		}
	}
</script>

<style>
	
</style>
