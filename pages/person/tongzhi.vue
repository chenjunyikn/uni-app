<template>
	<view>
		<view class="thead">
			<button @click="fn1">全选</button>
			<button @click="fn2">全不选</button>
			<button @click="fn3">删除</button>
			<button @click="fn4">标记为已读</button>
		</view>
		<view class="isuniload">
			<uni-load-more v-if="loadstatu" :content-text="loadoption" status="loading"></uni-load-more>
		</view>
		<checkbox-group @change="checkedupdate">
			<view class="list" v-for="obj in arr" :key='obj.msid'>
				<view class="list-item">
					<view class="checks">
						<checkbox :checked="checked||obj.checked" :value="obj.msid" />
					</view>
					<view class="text" @tap="find(obj.msid,obj.state)">
						<label class="title">{{obj.title}}</label>
						<label class="note">{{obj.contents}}</label>
					</view>
					<label class="time">
						<uni-badge :text="obj.senddates" type="success"></uni-badge>
					</label>
					<view class='dot' v-show="obj.state==2"></view>
				</view>
			</view>
		</checkbox-group>
		<uni-pagination :current='current' :pageSize='pageSize' :total="total" @change="changeCurrent"></uni-pagination>
	</view>
</template>

<script>
	import uniBadge from "@dcloudio/uni-ui/lib/uni-badge/uni-badge.vue"
	import uniLoadMore from "@dcloudio/uni-ui/lib/uni-load-more/uni-load-more.vue"
	import uniPagination from "@dcloudio/uni-ui/lib/uni-pagination/uni-pagination.vue"
	export default {
		components: {
			uniBadge,
			uniPagination,
			uniLoadMore
		},
		data() {
			return {
				userid: '',
				arr: [],
				loadstatu: false,
				checked: null, //全选控制
				msids: '' ,//记录已选中的msid
				current: 1, //当前页
				pageSize: 8, //每页数据量
				total: 0, //数据总量
			}
		},
		onLoad() {
			//在本地缓存中取值compid并给页面compid变量赋值
			//getStorage和request请求是异步交互，同时执行，所以将request嵌套getStorage
			uni.getStorage({
				key: 'userid',
				success: (res) => {
					this.userid = res.data;
					this.getdata();
				}
			})
		},
		methods: {
			changeCurrent(e) {
				//当前页即时更新
				this.current = e.current;
				this.checked=null;
				this.getdata();
			},
			getdata() {
				this.loadstatu = true;
				//后台获取需要的数据并给页面展示的数据赋值
				uni.request({
					url: this.url + 'boxMs/listByPage',
					data: {
						userid: this.userid,
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
						for (var i = 0; i < this.arr.length; i++) {
							if (this.arr[i].content.length >= 20) {
								this.arr[i].contents = this.arr[i].content.substring(0, 20) + '...';
								this.arr[i].senddates = this.arr[i].senddate.substring(this.arr[i].senddate.length-8,this.arr[i].senddate.length-3);
								this.arr[i].checked = false;
							}
						}
						this.loadstatu = false;
					}
				});
			},
			checkedupdate(e) {
				//获取选中的checkbox的value的数组
				var values = e.target.value;
				//定义标记
				var ised = false;
				for (var i = 0, lenI = this.arr.length; i < lenI; i++) {
					//取出对象
					const item = this.arr[i];
					//初始化标记
					ised = false;
					for (var j = 0; j < values.length; j++) {
						if (values[j] == item.msid) {
							//如果当前对象的msid在values数组中，标记为true
							ised = true;
							break;
						}
					}
					//更新对象的checked属性
					if (ised) {
						this.arr[i].checked = true;
					} else {
						this.arr[i].checked = false;
					}
				}
				//关闭全选控制
				this.checked = null;
				//将已选中的msid放入到msids中
				this.msids = values.join(",");
			},
			find(msid,state) {
				//如果当前为已读，直接跳转；否则置为已读成功后，跳转页面
				if (state == 3) {
					uni.navigateTo({
						url: "details?msid="+msid
					});
				} else {
					var date = new Date();
					date = date.toLocaleString().split(" ");
					date = date[0] + " " + date[1].substring(2, date[1].length);
					//后台获取需要的数据并给页面展示的数据赋值
					uni.request({
						url: this.url + 'boxMs/updateSelective',
						data: {
							msid: msid,
							state: 3,
							readdate: date
						},
						success: (res) => {
							//置为已读成功后，跳转页面
							uni.navigateTo({
								url: "details?msid="+msid
							});
							this.getdata();
						}
					});
				}
			},
			fn1() {
				//全选操作
				var msids = [];
				//将arr中所有对象的checked置为true
				for (var i = 0; i < this.arr.length; i++) {
					this.arr[i].checked = true;
					msids.push(this.arr[i].msid);
				}
				//开启全选控制，置为选中
				this.checked = true;
				//将已选中的msid放入到msids中
				this.msids = msids.join(",");
			},
			fn2() {
				//取消全选操作
				//将arr中所有对象的checked置为false
				for (var i = 0; i < this.arr.length; i++) {
					this.arr[i].checked = false;
				}
				//开启全选控制，置为未选中
				this.checked = false;
				//将msids置为空
				this.msids = '';
			},
			fn3() {
				//批量删除操作
				if(this.msids.length==0){
					uni.showToast({
						icon: 'none',
						title: '未选中',
						duration: 1000
					});
					return;
				}
				uni.request({
					url: this.url + 'boxMs/updateSelectives',
					data: {
						msids: this.msids,
						state: 7
					},
					success: (res) => {
						uni.showToast({
							icon: 'success',
							title: '删除成功',
							duration: 1000
						});
						this.checked=null;
						this.getdata();
					}
				})
			},
			fn4() {
				if(this.msids.length==0){
					uni.showToast({
						icon: 'none',
						title: '未选中',
						duration: 1000
					});
					return;
				}
				//批量已读操作
				var date = new Date();
				date = date.toLocaleString().split(" ");
				date = date[0] + " " + date[1].substring(2, date[1].length);
				uni.request({
					url: this.url + 'boxMs/updateSelectives',
					data: {
						msids: this.msids,
						state: 3,
						readdate: date
					},
					success: (res) => {
						this.checked=null;
						this.getdata();
					}
				})
			}
		}
	}
</script>

<style>
	.thead {
		padding-top: 10upx;
		background: #ECECEC;
	}

	.thead button {
		display: inline-block;
		width: 25%;
		height: 80upx;
		line-height: 80upx;
		font-size: 25upx;
	}

	.list {
		padding: 0upx 0upx 0upx 20upx;
	}

	.list-item {
		font-size: 30upx;
		padding: 30upx 0upx;
		border-bottom: 1upx solid #ECECEC;
	}

	.checks {
		display: inline-block;
		vertical-align: bottom;
	}

	.checks checkbox {
		transform: scale(0.7);
	}

	.text {
		display: inline-block;
	}

	.title {
		display: block;
	}

	.note {
		color: #888;
		font-size: 28upx;
	}

	.time {
		float: right;
		position: relative;
		top: 20upx;
		right: 25upx;
	}

	.dot {
		position: relative;
		top: -15upx;
		right: -85upx;
		float: right;
		height: 16upx;
		width: 16upx;
		border-radius: 100%;
		background: #dd524d;
	}

	.isuniload {
		position: absolute;
		top: 500upx;
		bottom: 0;
		left: 0;
		right: 0;
		margin: auto;
	}
</style>
