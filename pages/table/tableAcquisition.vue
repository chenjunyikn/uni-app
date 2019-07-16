<template>
	<view>
		<view class="div-table">
			<view class="thead">
				<view class="th">
					<view class="td">同步数据条数</view>
					<view class="td">同步时间</view>
					<view class="td">同步人</view>
				</view>
			</view>
			<view class="tr" v-for="obj in arrTable" :key='obj.index'>
				<view class="td">{{obj.synchroRows}}</view>
				<view class="td">{{obj.synchroDate}}</view>
				<view class="td">{{obj.cname}}</view>
			</view>
		</view>
		<uni-pagination :current='current' :pageSize='pageSize' :total="total" @change="changeCurrent"></uni-pagination>
	</view>
</template>

<script>
	import uniPagination from "@dcloudio/uni-ui/lib/uni-pagination/uni-pagination.vue"
	export default {
		components: {uniPagination},
		data() {
			return {
				userid:'',
				compid:'',
				arrTable:[],
				current:1, //当前页
				pageSize:6, //每页数据量
				total:0 //数据总量
			};
		},
		onLoad() {
			//在本地缓存中取值compid并给页面compid变量赋值
			//getStorage和request请求是异步交互，同时执行，所以将request嵌套getStorage
			uni.getStorage({
				key: 'userid',
				success: (res) => {
					this.userid = res.data;
				}
			});
			uni.getStorage({
				key: 'compid',
				success: (res) => {
					this.compid = res.data;
					//后台获取需要的数据并给页面展示的数据赋值
					uni.request({
						url: this.url + 'synchro/searchByPage',
						data: {
							userid: this.userid,
							compid: this.compid,
							currentPage:this.current,
							pageSize:this.pageSize
						},
						success: (res) => {
							//显示数据
							this.arrTable = res.data.tableData;
							//每页数据量
							this.pageSize = res.data.paging.pageSize;
							//数据总量
							this.total = res.data.paging.total;
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
		methods:{
			changeCurrent(e){
				//当前页即时更新
				this.current = e.current;
				uni.request({
					url: this.url + 'synchro/searchByPage',
					data: {
						userid: this.userid,
						compid: this.compid,
						currentPage:this.current,
						pageSize:this.pageSize
					},
					success: (res) => {
						//显示数据
						this.arrTable = res.data.tableData;
						//每页数据量
						this.pageSize = res.data.paging.pageSize;
						//数据总量
						this.total = res.data.paging.total;
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
