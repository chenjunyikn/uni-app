<template>
	<view>
		<view class="list-content">
			<radio-group @change="radioChange">
				<label class="list" v-for="(item, index) in items" :key="item.value">
					<view class="text">{{item.name}}</view>
					<view>
						<radio :value="item.value" :checked="index === current" />
					</view>
				</label>
			</radio-group>
		</view>
	</view>
</template>
<script>
	export default {
		data() {
			return {
				title: 'radio',
				items: [{
						value: 'USA',
						name: '经典蓝',
						color:'#0065FE',
						checked: 'true'
					},
					{
						value: 'CHN',
						name: '魅力红',
						color:'#c00'
					},
					{
						value: 'BRA',
						name: '活力蓝',
						color:'#287dfa'
					},
					{
						value: 'JPN',
						name: '孔雀绿',
						color:'#005A5C'
					},
					{
						value: 'BKQ',
						name: '布卡其',
						color:'#c3b091'
					},
					{
						value: 'NOH',
						name: '暖耦荷',
						color:'#e4c6d0'
					},
					{
						value: 'YXB',
						name: '月下白',
						color:'#d8ebef'
					},
					{
						value: 'NXS',
						name: '暖杏色',
						color:'#fbdab1'
					}
				],
				current: 2
			}
		},
		methods: {
			radioChange(evt) {
				for (var i = 0; i < this.items.length; i++) {
					if (this.items[i].value === evt.target.value) {
						this.current = i;
						uni.setNavigationBarColor({
							backgroundColor:this.items[i].color,
							frontColor:"#ffffff"
						});
						break;
					}
				}
			}
		}
	}
</script>

<style>
	.list-content{
		padding: 20upx;
		font-size: 30upx;
	}
	.list{
		display:flex;
		height: 100upx;
		line-height: 100upx;
		width: 100%;
		border-bottom:1px solid rgb(243,243,243);
	}
	.text{
		flex:1 1 auto;
	}
	
</style>