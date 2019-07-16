<template>
	<view scroll-y="true">
		<view class="h1">当前港口未来能耗比率趋势流向</view>
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
				compid:'',
				dateArr:[],
				ratioArr:[]
			}
		},
		methods: {
			lineInit(canvas, width, height) {
				lineChart = echarts.init(canvas, null, {
					width: width,
					height: height
				})
				canvas.setChart(lineChart)
				var option = {
					xAxis: {
						data: this.dateArr
					},
					yAxis: {
						type: 'value'
					},
					series: [{
						data: this.ratioArr,
						type: 'line'
					}]
				};
				this.getforecast(option);
			},
			getforecast(option){
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
								for(var obj of res.data){
									this.dateArr.push(obj.date.split("/")[1]);
									this.ratioArr.push(obj.ratio/100);
								}
								lineChart.setOption(option);
								return lineChart;
							}
						})
					}
				});
			}
		}
	}
</script>

<style>
	page{
		background: #ECECEC;
	}
	.h1{
		font-size: 35upx;
		font-weight: 700;
		text-align: center;
		padding-top: 40upx;
	}
	.chart-father {
		position: relative;
		left: 30upx;
		width: 100%;
		height: 500upx;
	}
</style>
