<template>
	<view>
		<!-- 当前日期 -->
		<view class="cdate">{{cdate}}</view>
		<!-- 总的能耗 -->
		<view class="total">
			<view class="left">{{totalEnergy}}</view>
			<view class="right">
				<view>总</view>
				<view>折标煤/W</view>
			</view>
		</view>
		<!-- 能耗分类展示 -->
		<view class="type">
			<view class="left">
				<view>
					<image src="http://xiuer.mengruo.top/shop/images/xiuer/images/water.png"></image>
				</view>
				<view>{{water}}/W</view>
				<view>水耗:吨</view>
			</view>
			<view class="middle">
				<view>
					<image src="http://xiuer.mengruo.top/shop/images/xiuer/images/ele.png"></image>
				</view>
				<view>{{ele}}/W</view>
				<view>电耗:kWh</view>
			</view>
			<view class="right">
				<view>
					<image src="http://xiuer.mengruo.top/shop/images/xiuer/images/oil.png"></image>
				</view>
				<view>{{oil}}/W</view>
				<view>油耗:L</view>
			</view>
		</view>
		<!-- 按钮 -->
		<view class="buttons">
			<button class="day" :style={background:bgColor1} @click="fn('day')">日</button>
			<button class="month" :style={background:bgColor2} @click="fn('month')">月</button>
		</view>
		<!-- 图表展示 -->
		<lineChart></lineChart>
	</view>
</template>

<script>
	import lineChart from "../charts/chartIndex.vue"
	export default {
		components: {
			lineChart
		},
		data() {
			return {
				cdate: '',
				bgColor1: '#1296db',
				bgColor2: '#ADADAD',
				totalEnergy: 0,
				compid: '',
				water: 0,
				ele: 0,
				oil: 0
			}
		},
		mounted() {
			this.calcDate();
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
						url: this.url + 'energyConsume/DailyConsume',
						data: {
							compid: this.compid
						},
						success: (res) => {
							var today = res.data[parseInt(this.cdate)];
							if(today){
								this.totalEnergy = (today.Consume/10000).toFixed(2);
								this.water = (today.waterConsume/10000).toFixed(2);
								this.ele = (today.electricConsume/10000).toFixed(2);
								this.oil = (today.oilConsume/10000).toFixed(2);
							}else{
								this.totalEnergy = 0;
								this.water = 0;
								this.ele = 0;
								this.oil = 0;
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
				}
			})
		},
		methods: {
			fn(type) {
				if (type == 'day') {
					this.bgColor1 = '#1296db';
					this.bgColor2 = '#ADADAD';
					this.calcDate();
					this.DayConsume();
				} else {
					this.bgColor1 = '#ADADAD';
					this.bgColor2 = '#1296db';
					this.calcMonth();
					this.MonthConsume();
				}
			},
			calcDate() {
				var tdate = new Date();
				var day = tdate.getDate(); //此方法为日
				this.cdate = day + "日";
			},
			calcMonth() {
				var tdate = new Date();
				var month = tdate.getMonth() + 1; //月份从零开始
				this.cdate = month + "月";
			},
			MonthConsume(){
				uni.request({
					url: this.url + 'energyConsume/MonthlyConsume',
					data: {
						compid: this.compid
					},
					success: (res) => {
						var today = res.data[this.cdate];
						if(today){
							this.totalEnergy = (today.Consume/10000).toFixed(2);
							this.water = (today.waterConsume/10000).toFixed(2);
							this.ele = (today.electricConsume/10000).toFixed(2);
							this.oil = (today.oilConsume/10000).toFixed(2);
						}else{
							this.totalEnergy = 0;
							this.water = 0;
							this.ele = 0;
							this.oil = 0;
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
			DayConsume(){
				uni.request({
					url: this.url + 'energyConsume/DailyConsume',
					data: {
						compid: this.compid
					},
					success: (res) => {
						var today = res.data[parseInt(this.cdate)];
						if(today){
							this.totalEnergy = (today.Consume/10000).toFixed(2);
							this.water = (today.waterConsume/10000).toFixed(2);
							this.ele = (today.electricConsume/10000).toFixed(2);
							this.oil = (today.oilConsume/10000).toFixed(2);
						}else{
							this.totalEnergy = 0;
							this.water = 0;
							this.ele = 0;
							this.oil = 0;
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
			}
		}
	}
</script>

<style>

</style>
