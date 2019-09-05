<template>
	<view  class="content">
		<view class="back">
			<image src="../../static/Back.png" 
			@click="back"
			style="height:50rpx;width:50rpx;"></image>
		</view>
		<view class="tx">
			<image src="../../static/tx.jpg" style="border: 1rpx black solid;border-radius: 200rpx;height:150rpx;width:150rpx;"></image>
		</view>
		<view class="text">
			<view class="login">
				<view class="login-item">
					<view class="title">账号</view>
					<input type="text"  class=""  placeholder="请输入用户名" v-model="userName">
				</view>
				<view class="login-item">
					<view class="title">密码</view>
					<input type="password"  class=""  placeholder="请输入密码" v-model="password">
				</view>
				<button class="btn" @click="toMyAccount(userName,password)">注册/登录</button>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				userName:"",//用户名
				password:"",//密码
				account:[],//用户数据
			}
		},
		methods: {
			back(){
				uni.navigateBack({
					delta: 1
				});
			},
			toMyAccount(name,pwd){
				if(!this.check(name,pwd)){
					return;
				}
				uni.request({
					url: `${this.$url}/user/registOrLogin?&&qq=40699904`,
					method: 'POST',
					data: {username:name,password:pwd},
					success: res => {
						this.account = res.data.data;
						console.log(res.data);
					},
					fail: () => {},
					complete: () => {}
				});
			},
			check(name,pwd){
				if(name===''){
					uni.showToast({
						title: '未填写用户名！'
					});
					return false;
				}
				else if(pwd===''){
					uni.showToast({
						title: '未填写密码！'
					});
					return false;
				}
				else{
					return true;
				}
			},
		}
	}
</script>

<style>
	.content{
		width: 100%;
	}
	.back{
		width:96%;
		height:50rpx;
		padding:2%;
	}
	.tx{
		width: 100%;
		height:400rpx;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	.login{
		width:90%;
		height:200rpx;
		padding:5%;
	}
	.login-item{
		display: flex;
		padding:15rpx;
		border: 0;
		border-bottom: solid 1px lavender;
	}
	.title{
		width:15%;
		height:50rpx;
		padding:0 4%;
		color: grey;
	}
	.text{
		display: flex;
		justify-content: center;
		align-items: center;
	}
	.btn{
		color:#ffffff;
		background: #007AFF;
		margin:10% 4%;
	}
</style>
