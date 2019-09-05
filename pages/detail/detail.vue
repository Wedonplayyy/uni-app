<template>
	<view style="width: 100%;">
		<view class="video">
			<video :poster="movieDetail.poster"
			  show-center-play-btn controls
			  :src="movieDetail.trailer"
			  style="width: 100%; height: 425rpx;"
			>
			<cover-image class="controls-back img" @click="back" src="../../static/arrowleft.png"></cover-image>
            <cover-image class="controls-share img" @click="share" src="../../static/share.png"></cover-image>
			</video>
		</view>
		<view class="info-text">
			<view class="cover">
				<image :src="movieDetail.cover" mode="aspectFit"></image>
			</view>
			<view class="info">
				<view class="title">{{movieDetail.name}}</view>
				<view class="text"> {{movieDetail.basicInfo}}</view>
				<view class="text">{{movieDetail.originalName}}</view>
				<view class="text">{{movieDetail.releaseDate}}</view>
				<view style="text-align: left;padding: 2%;display: flex;">
					<view>
						<view>综合评分：</view>
						<view style="text-align: center;padding: 4%;color: orange;">{{movieDetail.score}}</view>
					</view>
					<view class="text" style="display:flex;justify-content: center;align-items: center;">{{movieDetail.prisedCounts}}人点赞</view>
				</view>
			</view>
		</view>
		<hr style="width:90%;margin-left:5%;">
		<view class="intro">
			<view class="text">剧情介绍:</view>
			<view class="intro-text">{{movieDetail.plotDesc}}</view>
		</view>
		<hr style="width:90%;margin-left:5%;">
		<view class="intro">
			<view class="text">演职人员:</view>
			<view >
				<scroll-view scroll-x="true" class="scroll">
					<view class="photo">
						<image :src="staffDirector.photo"></image>
						<view class="text">{{staffDirector.name}}</view>
						<view class="text">导演</view>
					</view>
					<block v-for="item in staffPlayer" >
						<view class="photo"  >
							<image :src="item.photo" mode="aspectFit"></image>
							<view class="text">{{item.name}}</view>
							<view class="text">饰演:{{item.actName}}</view>
						</view>
						
					</block>
				</scroll-view>
			</view>
		</view>
		<hr style="width:90%;margin-left:5%;">
		<view class="intro">
			<view class="text">剧照:</view>
			<view class="plotPics">
				<view v-for="item in this.plotPics">
					<image :src="item" style="width:200rpx;height:240rpx;padding:5%;"></image>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				movieDetail:{},//预告片详情
				staffDirector:{},//导演
				staffPlayer:[],//演员
				plotPics:[],//剧照
			}
		},
		onLoad(option) {
			console.log(option.id);
			this.getDetail(option.id);
			this.getStaffDirector(option.id);
			this.getStaffPlayer(option.id);
			
		},
		methods: {
			back(e){//返回上一层
				uni.showToast({
					title: '返回'
				});
			},
			share(){//分享
				
			},
			getDetail(id){
				uni.request({
					url: `${this.$url}/search/trailer/`+id+`?qq=40699904`,
					method: 'POST',
					data: {},
					success: res => {
						this.movieDetail = res.data.data;
						console.log(res.data.data);
						this.plotPics = JSON.parse(this.movieDetail.plotPics);
						console.log(typeof(this.movieDetail.plotPics));
						console.log(this.plotPics);
						
					},
					fail: () => {},
					complete: () => {}
				});
			},
			getStaffDirector(id){
				uni.request({
					url: `${this.$url}/search/staff/`+id+`/1?qq=40699904`,
					method: 'POST',
					data: {},
					success: res => {
						this.staffDirector = res.data.data[0];
						console.log(this.staffDirector);
					},
					fail: () => {},
					complete: () => {}
				});
			},
			getStaffPlayer(id){
				uni.request({
					url: `${this.$url}/search/staff/`+id+`/2?qq=40699904`,
					method: 'POST',
					data: {},
					success: res => {
						this.staffPlayer = res.data.data;
						console.log(this.staffPlayer);
					},
					fail: () => {},
					complete: () => {}
				});
			},
		}
	}
</script>

<style>
	cover-image {
		display: inline-block;
	}
	.img {
		position: absolute;
		width: 50rpx;
		height: 50rpx;
		top: 14%;
		margin-top: -50rpx;
	}
	.controls-back {
	    left: 4rpx;
	}
	
	.controls-share {
	    right: 4rpx;
	}
	.info{
		width:50%;
	}
	.info-text{
		width:90%;
		padding: 8% 5% 2% 5%; 
		display: flex;
		justify-content: space-between;
	}
	image{
		width: 320rpx;
	}
	.title{
		padding:2%;
		text-align: left;
	}
	.text{
		padding:1% 2%;
		font-size: 26rpx;
		color:grey;
	}
	.intro{
		width: 90%;
		padding:2% 5%;
	}
	.intro-text{
		width:100%;
		padding:1% 2%;
		font-size: 80%;
		text-align: left;
	}
	.scroll{
		white-space: nowrap;
		width:1600rpx;
 	}
	.photo{
		display: inline-block;
		padding:10rpx;
	}
	.photo image{
		width:170rpx;
		height:220rpx;
	}
	.plotPics{
		display:flex;
		flex-wrap: wrap;
	}
</style>
