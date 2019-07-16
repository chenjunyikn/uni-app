<template>
	<view>
		<view class="text">
			<text>Q1：如何获得积分？</text><br>
			<text>部分城市每日签到可获得专享积分；</text><br>
			<view style="height: 40upx;"></view>
			<text>Q2：积分如何使用？</text><br>
			<text>积分仅限订单在线支付时使用</text><br>
			<text>此接口为模拟支付积分转换；</text><br>
			<text>积分仅限此平台使用，不能叠加或拆分使用；</text><br>
			<view style="height: 40upx;"></view>
			<text>Q3：下单的时候使用了积分，但是后来订单取消了，积分会返还吗？</text><br>
			<text>会返还，订单无效后积分会自动返还到您的账户里。</text><br>
			<view style="height: 40upx;"></view>
			<text>Q4：我的积分还未使用为什么失效了？</text><br>
			<text>积分的失效日期定位年底，请注意此信息。</text>
		</view>
	</view>
</template>

<script>
	import uniIcon from "@dcloudio/uni-ui/lib/uni-icon/uni-icon.vue"
	export default {
		components: {
			uniIcon
		},
		data() {
			return {
				
			}
		}
	}
</script>

<style>
	page{
		background-color: #F4F4F4;
	}
	.text{
		font-size: 30upx;
		margin: 30upx 6%;
	}
</style>
