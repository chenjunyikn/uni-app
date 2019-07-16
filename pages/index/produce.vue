<template>
	<view>
		<!-- 环形 -->
		<view class="produce">
			<view class="circle">
				<view class="font1">{{day}}<label class="text1">天</label></view>
				<label>安全用机</label><br>
				<label class="label">/</label>
				<view class="font2">{{getRepairInfo}}<label class="text1">条</label></view>
				<label>异常设备</label>
			</view>
		</view>
		<view class="produce1">
			<view class="text2">作业量统计</view>
			<view class="text3"><image src="http://xiuer.mengruo.top/shop/images/xiuer/images/produce.png"></image></view>
		</view>
		<!-- 列表 -->
		<view v-for="(obj,index) in flowArr" :key='index'>
			<view class="li" @click="charts(obj.flowid)">
				<label><uni-icon type="list" size="22" color='#43CD80'></uni-icon></label>
				<label>流程：{{obj.flowname}}</label>
				<label>作业量：{{obj.amount}}</label>
				<label class="icon"><uni-icon type="arrowright" size="22" color='#43CD80'></uni-icon></label>
			</view>
		</view>
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
				compid:'',
				day:'',
				getRepairInfo:'',
				flowArr:[]
			}
		},
		onLoad() {
			//在本地缓存中取值compid并给页面compid变量赋值
			//getStorage和request请求是异步交互，同时执行，所以将request嵌套getStorage
			uni.getStorage({
				key: 'compid',
				success: (res) => {
					this.compid = res.data;
					//后台获取需要的数据并给页面展示的数据赋值
					uni.request({
						url: this.url + 'produceRepair/getConsecutiveNormalDays',
						data: {
							compid: this.compid
						},
						success: (res) => {
							this.day = res.data;
						},
						fail: () => {
							uni.showToast({
								icon: 'none',
								title: '网络异常,请稍后重试',
								duration: 1000
							});
						}
					});
					uni.request({
						url: this.url + 'produceRepair/getRepairInfo',
						data: {
							compid: this.compid
						},
						success: (res) => {
							this.getRepairInfo = res.data.length;
						},
						fail: () => {
							uni.showToast({
								icon: 'none',
								title: '网络异常,请稍后重试',
								duration: 1000
							});
						}
					});
					uni.request({
						url: this.url + 'baseFlow/getTodayOfFlowAmount',
						data: {
							compid: this.compid
						},
						success: (res) => {
							this.flowArr = res.data;
						},
						fail: () => {
							uni.showToast({
								icon: 'none',
								title: '网络异常,请稍后重试',
								duration: 1000
							});
						}
					});
				}
			})
		},
		methods: {
			charts(flowid) {
				uni.navigateTo({
					url: "../charts/chartStatistics?compid="+this.compid+"&flowid="+flowid
				});
			}
		}
	}
</script>

<style>

</style>
