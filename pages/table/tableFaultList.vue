<template>
	<view>
		<view class="h1">本月机器故障记录汇总表</view>
		<view class="div-table">
			<view class="thead">
				<view class="th">
					<view class="td">设备名称</view>
					<view class="td">故障原因</view>
					<view class="td">报告人</view>
					<view class="td">故障时间(分钟)</view>
				</view>
			</view>
			<view class="tr" v-for="(obj,index) in arr" :key='index'>
				<view class="td">{{obj.devname}}</view>
				<view class="td">{{obj.cause}}</view>
				<view class="td">{{obj.faultuser}}</view>
				<view class="td">{{obj.faulttime}}</view>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			compid:'',
			arr:[]
		};
	},
	onLoad() {
		uni.getStorage({
			key: 'compid',
			success: (res) => {
				this.compid = res.data;
				//后台获取需要的数据并给页面展示的数据赋值
				uni.request({
					url: this.url + 'produceFault/search',
					data: {
						compid: this.compid
					},
					success: (res) => {
						this.arr = res.data;
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
};
</script>

<style lang="scss">
.h1{
	font-weight: 600;
	text-align: center;
	margin-top: 20upx;
}
$div-table-border-color: #888;
$div-table-border-width: 1upx;
.div-table {
	display: table;
	width: 100%;
	height: 100%;
	border-top: $div-table-border-width solid $div-table-border-color;
	border-bottom: $div-table-border-width solid $div-table-border-color;
	box-sizing: border-box;
	table-layout: fixed;
	position: relative;
	.celspan {
		display: table;
		width: 100%;
		height: 100%;
		.td {
			display: table-cell;
			border: none !important;
		}
		.td + .td {
			border-left: $div-table-border-width solid $div-table-border-color !important;
		}
	}
	.rowspan {
		display: table;
		width: 100%;
		height: 100%;
		.tr {
			display: table-row;
			border: none !important;
		}
	}

	&.fixed-thead {
		.tbody,
		.thead,
		.tr,
		.th,
		.td {
			display: block;
		}
		.tr,
		.th {
			&:after {
				content: '';
				display: block;
				visibility: hidden;
				clear: both;
				height: 0;
			}
		}
		.td {
			float: left;
			width: 33.33%;
		}
		.tbody {
			height: 400upx;
			overflow-y: auto;
			overflow-x: hidden;
		}
	}

	.tr,
	.th {
		display: table-row;
		& + .tr,
		& + .th {
			.td,
			.th {
				border-top: $div-table-border-width solid $div-table-border-color;
				word-break: break-word;
			}
		}
	}
	.td {
		background: #f0ffea;
		font-size: 25upx;
		display: table-cell;
		vertical-align: middle;
		text-align: center;
		box-sizing: border-box;
		padding: 30upx;
		&.noPadding {
			padding: 0;
		}
		& + .td {
			border-left: $div-table-border-width solid $div-table-border-color;
		}
	}
	.th .td {
		font-size: 30upx;
		font-weight: bold;
		background: #2c739d;
		color: white;
		font-weight: bold;
	}
	.thead {
		display: table-header-group;
	}
}
</style>
