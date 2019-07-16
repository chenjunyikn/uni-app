<template>
	<view>
		<input class="inp" type="text" v-model="value" :placeholder="text" />
		<button class="btn" type="primary" @click="update">保存</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				userid:'',
				text:'请输入修改内容',
				value:'',
				col:'username'
			}
		},
		//option为object类型，会序列化上个页面传递的参数
		onLoad:function (option) {
			this.col = option.col;
		},
		methods:{
			update(){
				uni.getStorage({
					key: 'userid',
					success: (res) => {
						this.userid = res.data;
						uni.request({
							url: this.url + 'baseUser/updateSelective?userid='+this.userid+"&"+this.col+"="+this.value,
							success: (res) => {
								uni.showToast({
									icon: 'none',
									title: '更改成功',
									duration: 1000
								});
								uni.navigateBack({
									delta: 2
								});
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
		}
	}
</script>

<style>
	.inp{
		border-bottom: 1upx solid #00CE47;
		margin: 60upx 5%;
		height: 80upx;
		font-size: 32upx;
	}
	.btn{
		margin: 50upx;
		height: 80upx;
		width: 200upx;
		font-size: 35upx;
		display: inline-block;
		background: #00CE47;
	}
</style>
