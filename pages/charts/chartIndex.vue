<template>
	<view scroll-y="true">
		<view class="h1">当前港口本月每天折标能耗展示</view>
		<view class="chart-father">
			<mpvue-echarts :echarts="echarts" :onInit="lineInit" canvasId="line" />
		</view>
	</view>
</template>

<script>
	import * as echarts from '../../components/demo-echart/echarts/echarts.simple.min.js';
	import mpvueEcharts from '../../components/demo-echart/mpvue-echarts/src/echarts.vue';
	var lineChart = null;
	export default {
		components: {
			mpvueEcharts
		},
		data() {
			return {
				echarts: echarts,
				compid: '',
				month: [],
				electricConsume: [],
				oilConsume: [],
				waterConsume: []
			}
		},
		onLoad() {
			
		},
		methods: {
			lineInit(canvas, width, height) {
				lineChart = echarts.init(canvas, null, {
					width: width,
					height: height
				})
				canvas.setChart(lineChart)
				var option = {
					 grid: {
						left: '3%',
						right: '4%',
						bottom: '3%',
						containLabel: true
					},
					toolbox: {
						feature: {
							saveAsImage: {}
						}
					},
					xAxis: {
						type: 'category',
						boundaryGap: false,
						data: this.month
					},
					yAxis: {
						type: 'value'
					},
					series: [
						{
							type:'line',
							data:this.waterConsume
						},
						{
							type:'line',
							data:this.electricConsume
						},
						{
							type:'line',
							data:this.oilConsume
						}
					]
				}
				this.DailyConsume(option);
			},
			DailyConsume(option){
				//在本地缓存中取值compid并给页面compid变量赋值
				//getStorage和request请求是异步交互，同时执行，所以将request嵌套getStorage
				uni.getStorage({
					key: 'compid',
					success: (res) => {
						this.compid = res.data;
						//后台获取需要的数据并给页面展示的数据赋值
						uni.request({
							url: this.url + 'energyConsume/DailyConsume',
							data: {
								compid: this.compid
							},
							success: (res) => {
								//遍历map集合类型数据
								for(var key in res.data){
									//得到key值暂时放入month数组
									this.month.push(key); //月份
									this.electricConsume.push(res.data[key].electricConsume); //今日电耗
									this.oilConsume.push(res.data[key].oilConsume); //今日油耗
									this.waterConsume.push(res.data[key].waterConsume); //今日水耗
								}
								lineChart.setOption(option);
								return lineChart;
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
				});
			}
		}
	}
</script>

<style>
	.h1{
		font-size: 35upx;
		font-weight: 600;
		text-align: center;
		margin-top: 40upx;
	}
	.chart-father {
		display: flex;
		width: 100%;
		height: 500upx;
		background: #FFFFFF;
		justify-content: center;
	}
</style>
