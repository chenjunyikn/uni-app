<template>
	<view scroll-y="true">
		<view class="h1">当前设备今日作业量信息饼图对比</view>
		<view class="data1"><label>斗轮机：</label>{{dlj}}KWh</view>
		<view class="data2"><label>装船机：</label>{{zcj}}KWh</view>
		<view class="data3"><label>皮带机：</label>{{pdj}}KWh</view>
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
				flowid:'',
				dlj:0,
				zcj:0,
				pdj:0
			}
		},
		//option为object类型，会序列化上个页面传递的参数
		onLoad: function (option) {
			this.compid = option.compid;
			this.flowid = option.flowid;
			uni.request({
				url: this.url + 'baseFlow/getTodayOfDeviceAmount',
				data: {
					compid: this.compid,
					flowid: this.flowid
				},
				success: (res) => {
					for(var key in res.data){
						this.dlj = res.data[key].dlj;
						this.zcj = res.data[key].zcj;
						this.pdj = res.data[key].pdj;
					}
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
			lineInit(canvas, width, height) {
				lineChart = echarts.init(canvas, null, {
					width: width,
					height: height
				})
				canvas.setChart(lineChart)
				var option = {
					series : [
						{
							name: '访问来源',
							type: 'pie',
							radius : '55%',
							center: ['50%', '60%'],
							data:[
								{value:this.zcj, name:'装船机'},
								{value:this.dlj, name:'斗轮机'},
								{value:this.pdj, name:'皮带机'}
							],
							itemStyle: {
								emphasis: {
									shadowBlur: 10,
									shadowOffsetX: 0,
									shadowColor: 'rgba(0, 0, 0, 0.5)'
								}
							}
						}
					]
				};
				lineChart.setOption(option);
				return lineChart;
			}
		}
	}
</script>

<style>
	.h1{
		font-size: 35upx;
		font-weight: 600;
		text-align: center;
		padding: 60upx;
	}
	.data1{
		width: 300upx;
		height: 60upx;
		line-height: 60upx;
		margin: 30upx 120upx;
		text-align: center;
		font-size: 30upx;
		color: #334b5c;
		font-weight: 600;
	}
	.data2{
		width: 300upx;
		height: 60upx;
		line-height: 60upx;
		margin: 30upx 120upx;
		text-align: center;
		font-size: 30upx;
		color: #c23531;
		font-weight: 600;
	}
	.data3{
		width: 300upx;
		height: 60upx;
		line-height: 60upx;
		margin: 30upx 120upx;
		text-align: center;
		font-size: 30upx;
		color: #61a0a8;
		font-weight: 600;
	}
	.chart-father {
		display: flex;
		margin-top: -100upx;
		width: 100%;
		height: 500upx;
		justify-content: center;
	}
</style>
