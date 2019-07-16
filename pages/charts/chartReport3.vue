<template>
	<view scroll-y="true">
		<view class="h1">{{year}}年设备类别作业量对比图表展示</view>
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
				year: new Date().getFullYear(),
				compid: '',
				month: [],
				compdata1: [],
				compdata2: [],
				compdata3: []
			}
		},
		methods: {
			lineInit(canvas, width, height) {
				lineChart = echarts.init(canvas, null, {
					width: width,
					height: height
				})
				canvas.setChart(lineChart);
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
					series: [{
							type: 'line',
							data: this.compdata1
						},
						{
							type: 'line',
							data: this.compdata2
						},
						{
							type: 'line',
							data: this.compdata3
						}
					]
				};
				this.getdata(option);
			},
			getdata(option) {
				uni.getStorage({
					key:'compid',
					success: (res) => {
						this.compid = res.data;
						uni.request({
							url: this.url + 'jobAmount/devTypeAmount',
							data: {
								compid: this.compid,
								year: this.year
							},
							success: (res) => {
								for (var name of res.data.rows) {
									this.month.push(name.月份);
									this.compdata1.push(name.装船机);
									this.compdata2.push(name.皮带机);
									this.compdata3.push(name.斗轮机);
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
				})
			}
		}
	}
</script>

<style>
	.h1 {
		font-size: 35upx;
		font-weight: 600;
		text-align: center;
		padding: 60upx;
	}

	.chart-father {
		display: flex;
		margin-top: -100upx;
		width: 100%;
		height: 500upx;
		background: #FFFFFF;
		justify-content: center;
	}
</style>
