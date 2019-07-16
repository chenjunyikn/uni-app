<template>
	<view>
		<lineforecast></lineforecast>
		<view class="thead">
			<view class="th"><label>日期</label></view>
			<view class="th"><label>港口名称</label></view>
			<view class="th"><label>能耗率</label></view>
			<view class="th"><label>评定等级</label></view>
		</view>
		<view class="prop" v-for="(obj,index) in arr" :key='index'>
			<view class="data">{{obj.date}}</view>
			<view class="devtype">{{obj.compname}}</view>
			<view class="ratio">{{obj.ratio}}%</view>
			<view class="grade"><uni-badge :text="obj.grade" :type="obj.grade=='优'?'success':obj.grade=='良'?'primary':obj.grade=='中'?'warning':'error'"></uni-badge></view>
		</view>
	</view>
</template>

<script>
	import uniBadge from "@dcloudio/uni-ui/lib/uni-badge/uni-badge.vue"
	import lineforecast from "../charts/chartforecast.vue"
	export default {
		components: {
			lineforecast,
			uniBadge
		},
		data() {
			return {
				arr:[],
				compid:''
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
						url: this.url + 'baseDevice/getforecast',
						data: {
							compid: this.compid
						},
						success: (res) => {
							this.arr = res.data;
						}
					})
				}
			})
		},
		methods: {

		}
	}
</script>

<style>
	page{
		font-size: 30upx;
	}
	.thead{
		border-bottom: 1upx solid #ECECEC;
		text-align: center;
		background: #FFFFFF;
	}
	.th{
		display: inline-block;
		padding: 30upx 0upx;
		width: 25%;
		font-weight: 700;
	}
	.prop{
		text-align: center;
		background: #FFFFFF;
		border-bottom: 1upx solid #ECECEC;
	}
	.data,.devtype,.ratio,.grade{
		display: inline-block;
		width: 25%;
		padding: 25upx 0upx;
	}
</style>
