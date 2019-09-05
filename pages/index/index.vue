<template>
	<view class="content">
		<view>
			<swiper indicator-color="#fff" indicator-active-color="grey" :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000"  style="width:100%;height:400rpx;">
				<swiper-item class="swiper-item" v-for="item in swiper" :id="item.id" :key="item.id">
					<view >
						<image :src="item.image" mode="" style="width:100%;height:400rpx;"></image>
					</view>
				</swiper-item>
			</swiper>
			<view class="title">热门超英</view>
			<view>
				<scroll-view scroll-x="true" class="scroll">
					<block v-for="item in superhero" :key="item.id" :id="item.id">
						<view class="img"  @click="toMovieDetail(item.id)">
							<image :src="item.cover" mode="aspectFit" ></image>
							<view style="font-size: 24rpx;overflow: hidden;white-space: nowrap;text-overflow: ellipsis;width:180rpx;">{{item.name}}</view>
							<view style="display: flex;">
								<uni-rate :value="(item.score)/2" disabled="true" size="14"></uni-rate>
								<view style="font-size: 24rpx">{{item.score}}</view>
							</view>
						</view>
					</block>
				</scroll-view>
			</view>
			<view class="title">热门预告</view>
			<view>
				<view class="grid">
					<view v-for="item in trailer" :key="item.id" style="display: flex;justify-content: center;padding:16rpx 0rpx;">
						<video :poster="item.cover"
						  show-center-play-btn controls
						  :src="item.trailer"
						  style="width: 90%; height: 225rpx;"
						/>
					</view>
				</view>
			</view>
			<view class="title">猜你喜欢</view>
			<view v-for="item in this.like" :id="item.id" :key="item.id">
				<view style="display: flex;justify-content: space-between;padding:20rpx 20rpx;">
					<image :src="item.cover" style="width:180rpx;height:240rpx;border-radius: 15rpx;"></image>
					<view style="padding: 0rpx 0rpx;width: 340rpx;">
						<view>{{item.name}}</view>
						<uni-rate :value="(item.score)/2" disabled size="14"></uni-rate>
						<view class="text">{{item.basicInfo}}</view>
						<view class="text">上映时间:{{timeFormat(item.createTime)}}</view>
					</view>
					<view></view>
					<view style="display: flex;justify-content: center;align-items: center;font-size: 30rpx;">赞一下</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import {uniRate} from "@dcloudio/uni-ui"
	export default {
		components:{
			uniRate
		},
		data() {
			return {
				swiper:[],//轮播图数据
				superhero:[],//热门超英
				trailer:[],//热门预告
				like:[],//猜你喜欢
			}
		},
		onLoad() {
			this.getSwiper();
			this.getSuperhero();
			this.getLike();
			this.getTrailer();
		},
		methods: {
			toMovieDetail(id){
				uni.navigateTo({
					url: '../../pages/detail/detail?id='+id,
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			},
			timeFormat(time){//格式化时间
				var date=new Date(time);
				var year=date.getFullYear();
				var month= date.getMonth()+1<10 ? "0"+(date.getMonth()+1) : date.getMonth()+1;
				var day=date.getDate()<10 ? "0"+date.getDate() : date.getDate();
				// 拼接
				return year+"-"+month+"-"+day;
			},
			getSwiper(){//获得轮播图数据
				uni.request({
					url: `${this.$url}/index/carousel/list?qq=929029017`,
					method: 'POST',
					data: {},
					success: res => {
						this.swiper = res.data.data;
						console.log(res.data)
					},
					fail: () => {},
					complete: () => {}
				});
			},
			getSuperhero(){//获得热门超英
				uni.request({
					url: `${this.$url}/index/movie/hot?qq=40699904&&type=superhero`,
					method: 'POST',
					data: {},
					success: res => {
						this.superhero = res.data.data;
						console.log(res.data);
					},
					fail: () => {},
					complete: () => {}
				});
			},
			getTrailer(){//热门预告
				uni.request({
					url: `${this.$url}/index/movie/hot?qq=40699904&&type=trailer`,
					method: 'POST',
					data: {},
					success: res => {
						this.trailer = res.data.data;
						console.log(res.data);
					},
					fail: () => {},
					complete: () => {}
				});
			},
			getLike(){//猜你喜欢
				uni.request({
					url: `${this.$url}/index/guessULike?qq=40699904`,
					method: 'POST',
					data: {},
					success: res => {
						this.like = res.data.data;
						console.log(res.data);
					},
					fail: () => {},
					complete: () => {}
				});
			},
		}
	}
</script>

<style>
	.content {
		width: 100%;
	}
	.scroll{
		white-space: nowrap;
		width: 1600rpx;
	}
	.title{
		padding:10rpx 8rpx;
	}
	.img{
		display: inline-block;
		padding:10rpx;
	}
	.img image{
		width:170rpx;
		height:220rpx;
	}
	.grid{
		display: grid;
		grid-template-columns: 50% 50%;
	}
	.text{
		font-size: 30rpx;
		color: grey;
		padding-top: 10rpx;
	}
</style>
