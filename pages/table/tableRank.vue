<template>
	<view>
		<view class="rank">
			<view class="second">
				<image src="http://xiuer.mengruo.top/shop/images/xiuer/images/RunnerUp.png"></image>
				<label>当前亚军</label>
				<label>{{RunnerUp}}</label>
			</view>
			<view class="first">
				<image src="http://xiuer.mengruo.top/shop/images/xiuer/images/Champion.png"></image>
				<label>当前冠军</label>
				<label>{{Champion}}</label>
			</view>
			<view class="third">
				<image src="http://xiuer.mengruo.top/shop/images/xiuer/images/SecondRunner.png"></image>
				<label>当前季军</label>
				<label>{{SecondRunner}}</label>
			</view>
		</view>
		<view class="div-table">
			<view class="thead">
				<view class="th">
					<view class="td">排名</view>
					<view class="td">港口名称</view>
					<view class="td">能耗率</view>
				</view>
			</view>
			<view class="tr" v-for="obj in rank" :key='obj.index'>
				<view class="td">{{obj.rnum}}</view>
				<view class="td">{{obj.compname}}</view>
				<view class="td">{{obj.ratio}}</view>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			RunnerUp:'未知',
			Champion:'未知',
			SecondRunner:'未知',
			compid:'',
			rank:[]
		}
	},
	onLoad() {
		uni.request({
			url: this.url + 'baseCompany/getEnergyConsumptionRanking',
			success: (res) => {
				this.rank = res.data;
				//在当天未同步状态没参数情况下处理undefined报错情况加个判断
				var data1 = res.data[0];
				var data2 = res.data[1];
				var data3 = res.data[2];
				if(data1){
					this.Champion = data1.compname;
				}
				if(data2){
					this.RunnerUp = data2.compname;
				}
				if(data3){
					this.SecondRunner = data3.compname;
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
};
</script>

<style lang="scss">
	page{
		font-size: 30upx;
	}
	.rank{
		height: 300upx;
		background: #3dc58f;
		text-align: center;
	}
	.rank view{
		display: inline-block;
		width: 120upx;
		height: 200upx;
		margin: 65upx;
	}
	.rank view image{
		width: 100upx;
		height: 100upx;
	}
	.rank view label{
		width: 33%;
		color: white;
	}
	$div-table-border-color: #E3E3E3;
	$div-table-border-width: 1upx;
	.div-table {
		display: table;
		width: 100%;
		height: 100%;
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
		.rowspan {
			.tr,
			.th {
				display: table-row;
				.td,
				.th {
					border: none;
				}
				& + .tr,
				& + .th {
					.td,
					.th {
						border-top: $div-table-border-width solid $div-table-border-color;
					}
				}
			}
		}
		.td {
			display: table-cell;
			vertical-align: middle;
			text-align: center;
			box-sizing: border-box;
			padding: 40upx;
			&.noPadding {
				padding: 0;
			}
		}
		.th .td {
			font-weight: bold;
		}
		.tbody {
			display: table-row-group;
		}
		.thead {
			display: table-header-group;
			.tr,
			.th {
				.td,
				.th {
					border-bottom: $div-table-border-width solid $div-table-border-color;
				}
			}
		}
		.tfoot {
			display: table-footer-group;
			.tr,
			.th {
				.td,
				.th {
					border-top: $div-table-border-width solid $div-table-border-color;
				}
			}
		}
		.colgroup {
			display: table-column-group;
		}
		.col {
			display: table-column;
		}
		.caption {
			display: table-caption;
		}
	}
</style>
