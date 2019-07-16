<template>
	<view>
		<uni-list>
			<uni-list-item title="港口能耗对比" show-extra-icon="true" :extra-icon="{color:'#1296db',size:'30',type:'navigate'}" @click="report(1)"></uni-list-item>
			<uni-list-item title="港口作业量对比" show-extra-icon="true" :extra-icon="{color:'#4cd964',size:'30',type:'navigate'}" @click="report(2)"></uni-list-item>
			<uni-list-item title="流程作业量对比" show-extra-icon="true" :extra-icon="{color:'#1296db',size:'30',type:'navigate'}" @click="report(3)"></uni-list-item>
			<uni-list-item title="设备类别作业量对比" show-extra-icon="true" :extra-icon="{color:'#4cd964',size:'30',type:'navigate'}" @click="report(4)"></uni-list-item>
			<uni-list-item title="设备作业量对比" show-extra-icon="true" :extra-icon="{color:'#1296db',size:'30',type:'navigate'}" @click="report(5)"></uni-list-item>
			<uni-list-item title="港口间设备利用率对比" show-extra-icon="true" :extra-icon="{color:'#4cd964',size:'30',type:'navigate'}" @click="report(6)"></uni-list-item>
			<uni-list-item title="港口间设备完好率对比" show-extra-icon="true" :extra-icon="{color:'#1296db',size:'30',type:'navigate'}" @click="report(7)"></uni-list-item>
			<uni-list-item title="成本核算信息" show-extra-icon="true" :extra-icon="{color:'#4cd964',size:'30',type:'navigate'}" @click="report(8)"></uni-list-item>
		</uni-list>
	</view>
</template>

<script>
	import uniList from '@dcloudio/uni-ui/lib/uni-list/uni-list.vue'
	import uniListItem from '@dcloudio/uni-ui/lib/uni-list-item/uni-list-item.vue'
	export default {
		components: {
			uniList,
			uniListItem
		},
		data() {
			return {
				
			}
		},
		onLoad() {

		},
		methods: {
			report(items){
				if(items==1){
					uni.navigateTo({
						url: "../charts/chartReport"
					});
				}else if(items==2){
					uni.navigateTo({
						url: "../charts/chartReport1"
					});
				}else if(items==3){
					uni.navigateTo({
						url: "../charts/chartReport2"
					});
				}else if(items==4){
					uni.navigateTo({
						url: "../charts/chartReport3"
					});
				}else if(items==5){
					uni.navigateTo({
						url: "../charts/chartReport4"
					});
				}else if(items==6){
					uni.navigateTo({
						url: "../charts/chartReport5"
					});
				}else if(items==7){
					uni.navigateTo({
						url: "../charts/chartReport6"
					});
				}else{
					uni.navigateTo({
						url: "../table/tableDevInfoList"
					});
				}
			}
		}
	}
</script>

<style>
	
</style>
