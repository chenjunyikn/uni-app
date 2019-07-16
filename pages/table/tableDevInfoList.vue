<template>
	<view>
		<view class="h1">{{year}}年成本核算信息表数据</view>
		<button class="btn" @click="lastYear">上一年</button>
		<button class="btn" @click="nextYear">下一年</button>
		<view class="label">[单位：吨]</view>
		<view class="div-table">
			<view class="thead">
				<view class="th">
					<view class="td">设备名称</view>
					<view class="td">作业量</view>
					<view class="td">能耗</view>
				</view>
			</view>
			<view class="isuniload"><uni-load-more v-if="loadstatu" status="loading"></uni-load-more></view>
			<view class="tr" v-for="(obj,index) in arr" :key='index'>
				<view class="td">{{obj.devname}}</view>
				<view class="td">{{obj.amount}}</view>
				<view class="td">{{obj.consume}}</view>
			</view>
		</view>
		<uni-pagination :current='current' :pageSize='pageSize' :total="total" @change="changeCurrent"></uni-pagination>
	</view>
</template>

<script>
	import uniPagination from "@dcloudio/uni-ui/lib/uni-pagination/uni-pagination.vue"
	import uniLoadMore from "@dcloudio/uni-ui/lib/uni-load-more/uni-load-more.vue"
	export default {
		components: {
			uniPagination,
			uniLoadMore
		},
		data() {
			return {
				compid: "",
				year: new Date().getFullYear(),
				current: 1, //当前页
				pageSize: 8, //每页数据量
				total: 0, //数据总量
				arr: [],
				loadstatu: false
			};
		},
		onLoad() {
			this.loadstatu = true;
			uni.getStorage({
				key: 'compid',
				success: (res) => {
					this.compid = res.data;
					//后台获取需要的数据并给页面展示的数据赋值
					uni.request({
						url: this.url + 'devInfo/costByPage',
						data: {
							compid: this.compid,
							year: this.year,
							currentPage: this.current,
							pageSize: this.pageSize
						},
						success: (res) => {
							//显示数据
							this.arr = res.data.tableData;
							//每页数据量
							this.pageSize = res.data.paging.pageSize;
							//数据总量
							this.total = res.data.paging.total;
							this.loadstatu = false;
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
			lastYear() { //上一年
				if (this.year != new Date().getFullYear() - 5) {
					--this.year;
				}
				this.current = 1;
				this.pageSize = 8;
				this.total = 0;
				this.getYear();
			},
			nextYear() { //下一年
				if (this.year != new Date().getFullYear()) {
					++this.year;
				}
				this.current = 1;
				this.pageSize = 8;
				this.total = 0;
				this.getYear();
			},
			getYear() {
				this.loadstatu = true;
				uni.request({
					url: this.url + 'devInfo/costByPage',
					data: {
						compid: this.compid,
						year: this.year,
						currentPage: this.current,
						pageSize: this.pageSize
					},
					success: (res) => {
						if (res.data.tableData != null) {
							this.arr = res.data.tableData;
							//每页数据量
							this.pageSize = res.data.paging.pageSize;
							//数据总量
							this.total = res.data.paging.total;
						} else {
							var message = {
								amount: '暂无数据'
							};
							this.arr = [];
							this.arr.push(message);
							//每页数据量
							this.pageSize = res.data.paging.pageSize;
							//数据总量
							this.total = res.data.paging.total;
						}
						this.loadstatu = false;
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
			changeCurrent(e) {
				this.loadstatu = true;
				//当前页即时更新
				this.current = e.current;
				uni.request({
					url: this.url + 'devInfo/costByPage',
					data: {
						compid: this.compid,
						currentPage: this.current,
						pageSize: this.pageSize
					},
					success: (res) => {
						//显示数据
						this.arr = res.data.tableData;
						//每页数据量
						this.pageSize = res.data.paging.pageSize;
						//数据总量
						this.total = res.data.paging.total;
						this.loadstatu = false;
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
	};
</script>

<style lang="scss">
	.isuniload{
		position:absolute;
		top:400upx;
		bottom:0;
		left:0;
		right:0;
		margin:auto;
	}
	.h1 {
		font-weight: 600;
		text-align: center;
		margin-top: 20upx;
	}

	.btn {
		width: 25%;
		height: 60upx;
		display: inline-block;
		font-size: 30upx;
		line-height: 60upx;
		border: 1upx #2c739d solid;
		color: #2c739d;
		margin: 30upx 0upx 30upx 20upx;
	}

	.label {
		display: inline-block;
		font-size: 30upx;
		position: relative;
		left: 150upx;
		bottom: 40upx;
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

			&+.tr,
			&+.th {

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

				&+.tr,
				&+.th {

					.td,
					.th {
						border-top: $div-table-border-width solid $div-table-border-color;
					}
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
			padding: 40upx;

			&.noPadding {
				padding: 0;
			}
		}

		.th .td {
			font-size: 30upx;
			font-weight: bold;
			background: #2c739d;
			color: white;
		}

		.tbody {
			display: table-row-group;
		}

		.thead {
			display: table-header-group;
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
