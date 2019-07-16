<template>
	<view scroll-y="true">
		<view class="h1">{{year}}年港口能耗对比图表展示</view>
		<view class="label">[单位：吨]</view>
		<view class="data1"><label>{{compname[0]}}</label><br><label>斗轮机：{{compdata1[0]}}</label><br><label>装船机：{{compdata2[0]}}</label><br><label>皮带机：{{compdata3[0]}}</label></view>
		<view class="data2"><label>{{compname[1]}}</label><br><label>斗轮机：{{compdata1[1]}}</label><br><label>装船机：{{compdata2[1]}}</label><br><label>皮带机：{{compdata3[1]}}</label></view>
		<view class="data3"><label>{{compname[2]}}</label><br><label>斗轮机：{{compdata1[2]}}</label><br><label>装船机：{{compdata2[2]}}</label><br><label>皮带机：{{compdata3[2]}}</label></view>
		<view class="data4"><label>{{compname[3]}}</label><br><label>斗轮机：{{compdata1[3]}}</label><br><label>装船机：{{compdata2[3]}}</label><br><label>皮带机：{{compdata3[3]}}</label></view>
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
				compname:[],
				compdata1:[],
				compdata2:[],
				compdata3:[]
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
					color: ['#EE7600'],
					tooltip : {
						trigger: 'axis',
						axisPointer : {
							type : 'shadow'
						}
					},
					grid: {
						left: '3%',
						right: '4%',
						bottom: '3%',
						containLabel: true
					},
					xAxis : [
						{
							type : 'category',
							data : this.compname,
							axisTick: {
								alignWithLabel: true
							}
						}
					],
					yAxis : [
						{
							type : 'value'
						}
					],
					series : [
						{
							name:'斗轮机',
							type:'bar',
							barWidth: '20%',
							data:this.compdata1
						},
						{
							name:'装船机',
							type:'bar',
							barWidth: '20%',
							data:this.compdata2
						},
						{
							name:'皮带机',
							type:'bar',
							barWidth: '20%',
							data:this.compdata3
						}
					]
				};
				this.getdata(option);
			},
			getdata(option){
				uni.request({
					url: this.url + 'devInfo/consume',
					data: {
						year: this.year
					},
					success: (res) => {
						for(var name of res.data.rows){
							this.compname.push(name.compname.substring(0,3));
							this.compdata1.push(name.斗轮机);
							this.compdata2.push(name.装船机);
							this.compdata3.push(name.皮带机);
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
		}
	}
</script>

<style>
	.h1 {
		font-weight: 600;
		text-align: center;
		margin-top: 20upx;
	}
	.label {
		font-size: 30upx;
		text-align: right;
		margin: 40upx 70upx;
	}
	.data1{
		display: inline-block;
		width: 335upx;
		height: 160upx;
		margin: 10upx 20upx;
		font-size: 30upx;
		color: #334b5c;
		font-weight: 600;
	}
	.data2{
		display: inline-block;
		width: 335upx;
		height: 160upx;
		margin: 10upx 20upx;
		font-size: 30upx;
		color: #c23531;
		font-weight: 600;
	}
	.data3{
		display: inline-block;
		width: 335upx;
		height: 160upx;
		margin: 10upx 20upx;
		font-size: 30upx;
		color: #EE7600;
		font-weight: 600;
	}
	.data4{
		display: inline-block;
		width: 335upx;
		height: 160upx;
		margin: 10upx 20upx;
		font-size: 30upx;
		color: #61a0a8;
		font-weight: 600;
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
