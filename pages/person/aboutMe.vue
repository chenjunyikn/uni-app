<template>
	<view>
		<view class="aboutMe">
			<image src="http://xiuer.mengruo.top/shop/images/xiuer/images/aboutMe.png" @click="fn"></image>
			<text class="about">能耗助手</text>
		</view>
		<view class="content">
			<view class="zan1" @click="fn1">
				<view class="zan">
					<image src="http://xiuer.mengruo.top/shop/images/xiuer/images/zan.png"></image>
				</view>
				<text>评个价</text>
			</view>
			<view class="cao1" @click="fn1">
				<view class="cao">
					<image src="http://xiuer.mengruo.top/shop/images/xiuer/images/cao.png"></image>
				</view>
				<text>吐个槽</text>
			</view>
		</view>
		<view class="list">
			<uni-list>
				<uni-list-item title="能耗助手用户须知" @click="user(1)"></uni-list-item>
				<uni-list-item title="能耗助手协议" @click="user(2)"></uni-list-item>
			</uni-list>
		</view>
		<view class="message">
			<text>能耗助手 最新版本</text>
			<text>1.0.0</text>
		</view>
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
		methods:{
			fn(){
				uni.showToast({
					icon: 'none',
					title: '官网下载最新版本',
					duration: 1000
				});
			},
			fn1(){
				uni.navigateTo({
					url: "uni-feedback"
				});
			},
			user(items){
				if(items==1){
					uni.navigateTo({
						url: "explain"
					});
				}else{
					uni.navigateTo({
						url: "disExplain"
					});
				}
			}
		}
	}
</script>

<style>
	page{
		font-size: 30upx;
		background-color: #f1f1f1;
		color: #707070;
	}
	.aboutMe{
		text-align: center;
		padding: 70upx;
		background-color: #FFFFFF;
	}
	.aboutMe image{
		height: 220upx;
		width: 220upx;
		border-radius: 30upx;
		border: 2upx solid #00BFFF;
	}
	.about{
		margin-top: 20upx;
		font-size: 35upx;
	}
	text{
		display: block;
	}
	.content{
		border: 1upx solid #ECECEC;
		background-color: #FFFFFF;
	}
	.zan1 text,.cao1 text{
		margin-top: 20upx;
	}
	.zan1,.cao1{
		display: inline-block;
		text-align: center;
		margin: 20upx 15%;
	}
	.zan,.cao{
		border: 5upx solid #707070;
		border-radius: 80upx;
		height: 130upx;
		width: 130upx;
	}
	.zan image{
		height: 100upx;
		width: 100upx;
		margin: 10upx;
	}
	.cao image{
		height: 100upx;
		width: 100upx;
		margin: 20upx;
	}
	.list{
		padding-top: 20upx;
	}
	.message{
		text-align: center;
		line-height: 60upx;
		margin-top: 380upx;
	}
</style>
