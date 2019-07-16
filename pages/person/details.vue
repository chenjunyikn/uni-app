<template>
	<view class="detail">
		<view class="sender">发件人：{{arr.sender}}</view>
		<view class="baseUser">收件人：{{arr.baseUser.cname}}</view>
		<view class="senddate">时间：{{arr.senddate}}</view>
		<view class="title">事件：{{arr.title}}</view>
		<view class="content">{{arr.content}}</view>
		<view class="footer">
			本消息为系统消息，请勿直接回复。
			如有疑问或建议，欢迎随时联系我们。
			Email：1967609601@qq.com
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				arr:{baseUser:{cname:""}}
			}
		},
		onLoad:function (option) {
			//后台获取需要的数据并给页面展示的数据赋值
			uni.request({
				url: this.url + 'boxMs/load',
				data: {
					msid: option.msid
				},
				success: (res) => {
					this.arr = res.data;
				}
			})
		},
		methods:{
			
		}
	}
</script>

<style>
	page{
		font-size: 30upx;
		background: #f0f8f0;
	}
	.detail{
		padding: 30upx;
		line-height: 60upx;
	}
	.content{
		border: 1upx solid #287dfa;
		background: #FFFFFF;
		border-radius: 20upx;
		padding: 20upx 40upx;
		line-height: 50upx;
		margin-top: 30upx;
	}
	.footer{
		position: fixed;
		bottom: 30upx;
		left: 30upx;
		right: 30upx;
		text-align: center;
	}
</style>
