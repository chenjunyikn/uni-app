<template>
	<view>
		<view class="thead">
			<view class="th"><label>今日排名</label></view>
			<view class="th"><label>设备名称</label></view>
			<view class="th"><label>达标指数</label></view>
		</view>
		<view v-for="(item,index) in items" :key="index">
			<view class="list" :style="{height:(index==isactive?200:55)+'px'}" @click="fn(index)">
				<view class="rank" :style="{color:(item.rank==1?'red':item.rank==2?'orange':item.rank==3?'dodgerblue':'black')}">{{item.rank}}</view>
				<view class="devname">{{item.devname}}</view>
				<view class="conditions" :style="{color:(item.rank==1?'red':item.rank==2?'orange':item.rank==3?'dodgerblue':'black')}">{{item.conditions}}</view>
				<view class="shop"><image :src="'http://xiuer.mengruo.top/shop/images/xiuer/images/'+(item.devtype=='斗轮机'?'dlj':item.devtype=='装船机'?'zcj':'pdj')+'.jpg'"></image><label>机型：{{item.devtype}}</label></view>
				<view class="shop_right">
					<view class="compare"><label><uni-badge text="是否合格" type='success'></uni-badge></label>{{item.compare==1?'合格':'不合格'}}</view>
					<view class="cause"><label><uni-badge text="故障原因" type='warning'></uni-badge></label>{{item.cause==null?'无':item.cause}}</view>
					<view class="ratio"><label><uni-badge text="能耗比率" type='primary'></uni-badge></label>{{item.ratio}}</view>
					<view class="message"><label><uni-badge text="诊断建议" type='default'></uni-badge></label>{{item.compare==1?'推荐使用':'建议弃用'}}</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import uniBadge from "@dcloudio/uni-ui/lib/uni-badge/uni-badge.vue"
	export default {
		 components: {uniBadge},
		data() {
			return {
				compid:'',
				isactive:null,
				items:[]
			}
		},
		onLoad:function (option) {
			//在本地缓存中取值compid并给页面compid变量赋值
			//getStorage和request请求是异步交互，同时执行，所以将request嵌套getStorage
			uni.getStorage({
				key: 'compid',
				success: (res) => {
					this.compid = res.data;
					//后台获取需要的数据并给页面展示的数据赋值
					uni.request({
						url: this.url + 'baseDevice/getYesterdayOfquadState',
						data: {
							compid: this.compid
						},
						success: (res) => {
							this.items = res.data;
							//option为object类型，会序列化上个页面传递的参数
							for (var i = 0; i < this.items.length; i++) {
								if(this.items[i].devid==option.devid){
									this.isactive=i;
									break;
								}
							}
						}
					})
				}
			})
		},
		methods:{
			fn(index){
				this.isactive=this.isactive==index?null:index;
			}
		}
	}
</script>

<style>
	page{
		font-size: 32upx;
	}
	.thead{
		border-bottom: 1upx solid #D3D3D3;
		text-align: center;
		background: #ECECEC;
	}
	.th{
		display: inline-block;
		padding: 5% 0upx;
		width: 33%;
		font-weight: 600;
		border-right: 1upx solid #D3D3D3;
	}
	.list{
		text-align: center;
		border-bottom: 1upx solid #ECECEC;
		overflow: hidden;
	}
	.rank,.devname,.conditions{
		display: inline-block;
		padding: 5% 0upx;
		width: 33.3%;
		border-bottom: 1upx solid #ECECEC;
	}
	.shop{
		height: 63%;
		width: 32%;
		display: inline-block;
		background: black;
		vertical-align: bottom;
	}
	.shop image{
		width: 242upx;
		height: 200upx;
	}
	.shop label{
		color: white;
		line-height: 50upx;
	}
	.shop_right{
		width: 68%;
		display: inline-block;
	}
	.compare,.ratio,.cause,.message{
		display: inline-block;
		padding: 5% 0upx;
		width: 50%;
	}
	.list view label{
		display: block;
	}
</style>
