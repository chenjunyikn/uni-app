<template>
	<view>
		<uni-notice-bar show-close="true" speed="50" show-icon="true" scrollable="true" single="true" :text="text"></uni-notice-bar>
		<wuc-tab textFlex :tab-list="tabList" :tabCur.sync="TabCur" @change="tabChange"></wuc-tab>
		<view v-show="TabCur === 0">
			<energy></energy>
		</view>
		<view v-show="TabCur === 1">
			<produce></produce>
		</view>
		<view v-show="TabCur === 2">
			<tableRank></tableRank>
		</view>
	</view>
</template>

<script>
	import uniNoticeBar from "@dcloudio/uni-ui/lib/uni-notice-bar/uni-notice-bar.vue"
	import WucTab from '@/components/wuc-tab/wuc-tab.vue'
	import energy from "../index/energy.vue"
	import produce from "../index/produce.vue"
	import tableRank from "../table/tableRank.vue"
	export default {
		components: {
			uniNoticeBar,
			WucTab,
			energy,
			produce,
			tableRank
		},
		data() {
			return {
				TabCur: 0,
				tabList: [{ name: '能耗信息' }, { name: '产销模型' }, { name: '港口能耗排名' }],
				text:'签到送积分，积分换礼品，各种豪华礼品大奖等你来兑，快来加入吧！'
			}
		},
		methods: {
			tabChange(index) {
				this.TabCur = index;
			}
		},
		//监听原生标题栏扩展按钮点击事件
		onNavigationBarButtonTap() {
			uni.navigateTo({
				url: '../index/forecast'
			});
		}
	}
</script>

<style>
	@import url("../../static/css/index.css");
</style>
