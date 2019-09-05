<template>
	<view>
		<view class="background">
			<view style="width:80%;height:30%;">
				<view style="display: flex;width:100%;height: 100%;">
					<image src="../../static/tx.jpg" style="border-radius: 200rpx;height:150rpx;width:150rpx;"></image>
					<view class="text" @click="toLgoin">注册/登录</view>
				</view>
				
			</view>
			
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			toLgoin(){
				uni.navigateTo({
					url: '../../pages/login/login',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			}
		}
	}
</script>
<style>
	.background{
		background-color: orange;
		width:100%;
		height:400rpx;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	.text{
		display: flex;
		width:250rpx;
		justify-content: center;
		align-items: center;
	}
</style>
