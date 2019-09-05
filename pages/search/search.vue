<template>
	<view>
		<view class="search">
			<uni-icon type="search" size="26"></uni-icon>
			<input v-model="value" type="text" class="search_input" placeholder="请输入电影信息" placeholder-style="font-size:40rpx;" @confirm="search"/>
			<view @click="search">搜索</view>
		</view>
		<view v-if="show">
			<view v-if="this.result.length!==0" class="grid">
				<view v-for="item in result" class="grid-item"  @click="toMovieDetail(item.id)">
					<image :src="item.cover" class="img"></image>
					<view style="padding:10rpx 0rpx;text-align: center;font-size: 30rpx;overflow: hidden;white-space: nowrap;text-overflow: ellipsis;width:100%;">{{item.name}}</view>
				</view>
			</view>
			<view v-else>
				<view style="width:100%;height:600rpx;display: flex;justify-content: center;align-items: center;">没有找到你想要的~</view>
			</view>
		</view>
	</view>
</template>

<script>
	import {uniIcon} from "@dcloudio/uni-ui"
	export default {
		components:{
			uniIcon
		},
		data() {
			return {
				pageSize : 12,//一次搜索数量
				value:"",//搜索内容
				result:[],//搜索结果
				show:false,//显示搜索结果
			}
		},
		onLoad() {
		},
		onShow() {
			// this.value='';
			// this.show = false;
		},
		onReachBottom(){
			this.pageSize+=12;
			console.log(this.pageSize);
			this.search();
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
			search(){//搜索
				uni.request({
					url: `${this.$url}/search/list?page=1&&pageSize=`+this.pageSize+`&&keywords=`+this.value+`&&qq=40699904`,
					method: 'POST',
					data: {},
					success: res => {
						console.log(res.data.data);
						this.result = res.data.data.rows
						console.log(this.result);
						this.show = true;
					},
					fail: () => {},
					complete: () => {}
				});
			},
		}
	}
</script>

<style>
	.search {
		display: flex;
		padding: 20rpx 30rpx;
	}
	.search_input{
		width:500rpx;
		height:50rpx;
		padding: 0 20rpx;
	}
	.img{
		width:200rpx;
		height:280rpx;
	}
	.grid{
		display: grid;
		grid-template-columns:  repeat(3, 33.3%);;
	}
	.grid-item{
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		padding:20rpx 0rpx;
	}
</style>
