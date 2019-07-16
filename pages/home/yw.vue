<template>
	<view>
		<wuc-tab textFlex :tab-list="tabList" :tabCur.sync="TabCur" @change="tabChange"></wuc-tab>
		<view v-show="TabCur === 0">
			<Acquisition></Acquisition>
		</view>
		<view v-show="TabCur === 1">
			<MailList></MailList>
		</view>
		<view v-show="TabCur === 2">
			<FaultList></FaultList>
		</view>
	</view>
</template>

<script>
	import WucTab from '@/components/wuc-tab/wuc-tab.vue'
	import MailList from '../yw/MailList.vue'
	import Acquisition from '../yw/Acquisition.vue'
	import FaultList from '../yw/FaultList.vue'
	export default {
		components:{WucTab,MailList,Acquisition,FaultList},
		data() {
			return {
				TabCur: 0,
				tabList: [{name:'数据采集'},{name:'通讯录'},{name:'故障记录'}]
			}
		},
		onLoad() {

		},
		methods: {
			tabChange(index) {
				this.TabCur = index;
			}
		}
	}
</script>

<style>
	@import url("../../static/css/yw.css");
</style>
