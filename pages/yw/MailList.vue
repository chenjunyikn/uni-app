<template>
	<view>
		<mSearch :show="false" border="1px #BEBEBE solid" :placeholder="count" @search="search"></mSearch>
		<view v-for="(obj,index) in user" :key='index'>
			<view class="ul" @click="MailList(obj.telno)">
				<view class="icon1"><uni-icon type="person-filled" size="35" color='#4cd964'></uni-icon></view>
				<view class="li">
					<view><label>姓名：{{obj.cname}}</label></view>
					<view><label class="phone">电话：{{obj.telno}}</label></view>
				</view>
				<view class="icon2"><uni-icon type="arrowright" size="22" color='#4cd964'></uni-icon></view>
			</view>
		</view>
	</view>
</template>

<script>
	import mSearch from '@/components/mehaotian-search/mehaotian-search.vue'
	import uniIcon from "@dcloudio/uni-ui/lib/uni-icon/uni-icon.vue"
	export default {
		components: {
			mSearch,
			uniIcon
		},
		data() {
			return {
				compid:'',
				count:'',
				user:[]
			};
		},
		onLoad() {
			uni.getStorage({
				key: 'compid',
				success: (res) => {
					this.compid = res.data;
					//后台获取需要的数据并给页面展示的数据赋值
					uni.request({
						url: this.url + 'baseUser/search',
						data: {
							compid: this.compid
						},
						success: (res) => {
							this.user = res.data;
							this.count = "在"+res.data.length+"位联系人中搜索";
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
			 search(e) {
				 uni.request({
				 	url: this.url + 'baseUser/search',
				 	data: {
				 		compid: this.compid,
				 		telno: e
				 	},
				 	success: (res) => {
				 		this.user = res.data;
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
			 MailList(phone){
				 uni.makePhoneCall({
					phoneNumber: phone //仅为示例
				});
			 }
		}
	}
</script>

<style>
	.ul{
		border-top: 1upx solid #E8E8E8;
	}
	.li{
		display: inline-block;
		height: 100upx;
		vertical-align: top;
		line-height: 50upx;
	}
	.li label{
		margin: 0upx 30upx;
	}
	.icon1{
		display: inline-block;
		position: relative;
		top: 10upx;
		float: left;
	}
	.icon2{
		display: inline-block;
		position: relative;
		top: 30upx;
		right: 20upx;
		float: right;
	}
	.phone{
		color: #929292;
	}
</style>
