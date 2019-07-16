<template>
	<view class="content">
		<uni-notice-bar speed="50" show-icon="true" scrollable="true" single="true" :text="text"></uni-notice-bar>
		<view class="jiqi">
			<label class="label1">No.1</label>
			<image src="http://xiuer.mengruo.top/shop/images/xiuer/images/gangkou1.jpeg"></image>
		</view>
		<view class="infoJq">
			<view class="message">
				<label>港口名称</label>
				<uni-badge type="primary" text="图示"></uni-badge><br>
				<label>{{compAnalysis[0].compname}}</label>
			</view>
			<view class="message">
				<label>达标设备数量</label><br>
				<label>{{compAnalysis[0].standcount}}</label>
			</view>
			<view class="message">
				<label>日达标率</label>
				<uni-badge :type="comppro[0].dayReachingCount" :text="comppro[0].dayReaching"></uni-badge><br>
				<label>{{compAnalysis[0].standratio}}</label>
			</view>
			<view class="message">
				<label>日能耗率</label>
				<uni-badge :type="comppro[0].dayType" :text="compAnalysis[0].evaluate"></uni-badge><br>
				<label>{{compAnalysis[0].ratio}}</label>
			</view>
			<view class="message1">
				<label>检测报告结果</label>
				<uni-badge :type="comppro[0].dayType" :text="compAnalysis[0].evaluate"></uni-badge><br>
				<label>{{comppro[0].message}}</label>
			</view>
		</view>
		<view class="jiqi">
			<label class="label2">No.2</label>
			<image src="http://xiuer.mengruo.top/shop/images/xiuer/images/gangkou2.jpg"></image>
		</view>
		<view class="infoJq">
			<view class="message">
				<label>港口名称</label>
				<uni-badge type="success" text="机型"></uni-badge><br>
				<label>{{compAnalysis[1].compname}}</label>
			</view>
			<view class="message">
				<label>达标设备数量</label><br>
				<label>{{compAnalysis[1].standcount}}</label>
			</view>
			<view class="message">
				<label>日达标率</label>
				<uni-badge :type="comppro[1].dayReachingCount" :text="comppro[1].dayReaching"></uni-badge><br>
				<label>{{compAnalysis[1].standratio}}</label>
			</view>
			<view class="message">
				<label>日能耗率</label>
				<uni-badge :type="comppro[1].dayType" :text="compAnalysis[1].evaluate"></uni-badge><br>
				<label>{{compAnalysis[1].ratio}}</label>
			</view>
			<view class="message1">
				<label>检测报告结果</label>
				<uni-badge :type="comppro[1].dayType" :text="compAnalysis[1].evaluate"></uni-badge><br>
				<label>{{comppro[1].message}}</label>
			</view>
		</view>
		<view class="jiqi">
			<label class="label3">No.3</label>
			<image src="http://xiuer.mengruo.top/shop/images/xiuer/images/gangkou3.jpg"></image>
		</view>
		<view class="infoJq">
			<view class="message">
				<label>港口名称</label>
				<uni-badge type="success" text="机型"></uni-badge><br>
				<label>{{compAnalysis[2].compname}}</label>
			</view>
			<view class="message">
				<label>达标设备数量</label><br>
				<label>{{compAnalysis[2].standcount}}</label>
			</view>
			<view class="message">
				<label>日达标率</label>
				<uni-badge :type="comppro[2].dayReachingCount" :text="comppro[2].dayReaching"></uni-badge><br>
				<label>{{compAnalysis[2].standratio}}</label>
			</view>
			<view class="message">
				<label>日能耗率</label>
				<uni-badge :type="comppro[2].dayType" :text="compAnalysis[2].evaluate"></uni-badge><br>
				<label>{{compAnalysis[2].ratio}}</label>
			</view>
			<view class="message1">
				<label>检测报告结果</label>
				<uni-badge :type="comppro[2].dayType" :text="compAnalysis[2].evaluate"></uni-badge><br>
				<label>{{comppro[2].message}}</label>
			</view>
		</view>
		<view class="jiqi">
			<label class="label4">No.4</label>
			<image src="http://xiuer.mengruo.top/shop/images/xiuer/images/gangkou4.jpg"></image>
		</view>
		<view class="infoJq">
			<view class="message">
				<label>港口名称</label>
				<uni-badge type="success" text="机型"></uni-badge><br>
				<label>{{compAnalysis[3].compname}}</label>
			</view>
			<view class="message">
				<label>达标设备数量</label><br>
				<label>{{compAnalysis[3].standcount}}</label>
			</view>
			<view class="message">
				<label>日达标率</label>
				<uni-badge :type="comppro[3].dayReachingCount" :text="comppro[3].dayReaching"></uni-badge><br>
				<label>{{compAnalysis[3].standratio}}</label>
			</view>
			<view class="message">
				<label>日能耗率</label>
				<uni-badge :type="comppro[3].dayType" :text="compAnalysis[3].evaluate"></uni-badge><br>
				<label>{{compAnalysis[3].ratio}}</label>
			</view>
			<view class="message1">
				<label>检测报告结果</label>
				<uni-badge :type="comppro[3].dayType" :text="compAnalysis[3].evaluate"></uni-badge><br>
				<label>{{comppro[3].message}}</label>
			</view>
		</view>
		<uni-drawer :visible="isdrawer">
			<uni-collapse>
				<uni-collapse-item v-if="devArr.length>0" :title='devArr[0].typename'>
					<view class="devtype" @click="jump(obj.devid)" v-for="(obj,index) in devArr[0].device" :key="index">{{obj.devname}}</view>
				</uni-collapse-item>
				<uni-collapse-item v-if="devArr.length>1" :title='devArr[1].typename'>
					<view class="devtype" @click="jump(obj.devid)" v-for="(obj,index) in devArr[1].device" :key="index">{{obj.devname}}</view>
				</uni-collapse-item>
				<uni-collapse-item v-if="devArr.length>2" :title='devArr[2].typename'>
					<view class="devtype" @click="jump(obj.devid)" v-for="(obj,index) in devArr[2].device" :key="index">{{obj.devname}}</view>
				</uni-collapse-item>
			</uni-collapse>
		</uni-drawer>
	</view>
</template>

<script>
	import {uniCollapse,uniCollapseItem} from "@dcloudio/uni-ui"
	import uniDrawer from "@dcloudio/uni-ui/lib/uni-drawer/uni-drawer.vue"
	import uniNoticeBar from "@dcloudio/uni-ui/lib/uni-notice-bar/uni-notice-bar.vue"
	import uniBadge from "@dcloudio/uni-ui/lib/uni-badge/uni-badge.vue"
	export default {
		components: {
			uniNoticeBar,
			uniBadge,
			uniDrawer,
			uniCollapse,
			uniCollapseItem
		},
		data() {
			return {
				isdrawer:false,
				devArr:[],
				text: '根据企业能耗率取前四排名，企业资源消耗越少，能耗率越小，排名就越靠前，所有详情企业排名见主页港口能耗排名。',
				compid: -1,
				compAnalysis: [{}, {}, {}, {}],
				comppro: [{
					dayReaching: '',
					dayReachingCount: '',
					dayType: '',
					message: ''
				}, {
					dayReaching: '',
					dayReachingCount: '',
					dayType: '',
					message: ''
				}, {
					dayReaching: '',
					dayReachingCount: '',
					dayType: '',
					message: ''
				}, {
					dayReaching: '',
					dayReachingCount: '',
					dayType: '',
					message: ''
				}]
			}
		},
		onHide() {
			this.isdrawer=false;
		},
		onLoad() {
			uni.showToast({
				icon: 'loading',
				title: '加载中',
				duration: 2300
			});
			this.getdata();
			//在本地缓存中取值compid并给页面compid变量赋值
			//getStorage和request请求是异步交互，同时执行，所以将request嵌套getStorage
			uni.getStorage({
				key: 'compid',
				success: (res) => {
					this.compid = res.data;
					//后台获取需要的数据并给页面展示的数据赋值
					uni.request({
						url: this.url + 'baseDevtype/findlist',
						data: {
							compid: this.compid
						},
						success: (res) => {
							this.devArr = res.data;
						}
					})
				}
			})
		},
		methods: {
			getdata() {
				uni.request({
					url: this.url + 'baseFlow/getquadEvaluations',
					data: {
						compid: this.compid,
						row: this.compAnalysis.length
					},
					success: (res) => {
						this.compAnalysis = res.data;
						for (var i in this.compAnalysis) {
							var fen = parseInt(this.compAnalysis[i].standratio);
							if (fen >= 80) {
								this.comppro[i].dayReaching = '优秀';
								this.comppro[i].dayReachingCount = 'primary';
							} else if (fen >= 60) {
								this.comppro[i].dayReaching = '良好';
								this.comppro[i].dayReachingCount = 'success';
							} else if (fen >= 40) {
								this.comppro[i].dayReaching = '一般';
								this.comppro[i].dayReachingCount = 'default';
							} else if (fen >= 20) {
								this.comppro[i].dayReaching = '警告';
								this.comppro[i].dayReachingCount = 'warning';
							} else {
								this.comppro[i].dayReaching = '危险';
								this.comppro[i].dayReachingCount = 'error';
							}
							if (this.compAnalysis[i].evaluate == '良好') {
								this.comppro[i].dayType = 'success';
								this.comppro[i].message = '恭喜你，今日您的机型各项指标都超常发挥，打败了各个港口' + this.compAnalysis[i].standratio + '的机器';
							} else if (this.compAnalysis[i].evaluate == '一般') {
								this.comppro[i].dayType = 'default';
								this.comppro[i].message = '恭喜你，今日您的机型各项指标都属于正常范围，打败了各个港口' + this.compAnalysis[i].standratio + '的机器';
							} else if (this.compAnalysis[i].evaluate == '较差') {
								this.comppro[i].dayType = 'warning';
								this.comppro[i].message = '提示，今日您的机型各项指标不在状态，仅打败了各个港口' + this.compAnalysis[i].standratio + '的机器';
							} else if (this.compAnalysis[i].evaluate == '极差') {
								this.comppro[i].dayType = 'error';
								this.comppro[i].message = '警告，今日您的机型各项指标无法正常运行，打败了各个港口' + this.compAnalysis[i].standratio + '的机器，请检查原因';
							} else {
								this.comppro[i].dayType = 'default';
								this.comppro[i].message = '今日无运行状况';
							}
						}
					}
				})
			},
			jump(devid){
				uni.navigateTo({
					url: '../fx/devType?devid='+devid
				});
			}
		},
		//监听原生标题栏左侧扩展按钮点击事件
		onNavigationBarButtonTap() {
			this.isdrawer = !this.isdrawer;
		}
	}
</script>

<style>
	@import url("../../static/css/fx.css");
</style>
