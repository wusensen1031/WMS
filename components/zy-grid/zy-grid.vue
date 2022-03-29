<template>
	<view>
		<view class="grid-title">
			<view></view>
			<text>{{title}}</text>
		</view>
		<view class="grid">
			<view class="grid-item" v-for="(item, index) in gridList" :key="index" @click="gridClick(item)">
				<view><image :src="item.imgUrl" mode=""></image></view>
				<view v-text="item.name"></view>
				<text v-if="item.tips" v-text="item.tips"></text>
			</view>
		</view>
	</view>
</template>

<script>
	export default{
		name: 'zy-grid',
		props: {
			col: {
				type: Number,
				value: 3
			},
			showTip: {
				type: Boolean,
				value: true
			},
			gridList: {
				type: Array,
				value: []
			},
			title:{
				type:String,
				value:"功能台"
			}
		},
		methods:{
			gridClick (item) {
				if(item.path){
					item.click = true;
					setTimeout(function() {
						item.click = false;
						uni.navigateTo({
						    url: item.path
						});
					}, 400);
				}
			}
		}
	}
</script>

<style lang="less" scoped>
	.grid-title{
		display: flex;
		align-items: center;
		font-size: 34rpx;
		color: rgba(0,0,0,.63);
		padding: 30upx 0;
		padding-bottom: 10rpx;
		width: 95%;
		margin: 0 2.5%;
		view{
			width: 15upx;
			height: 30upx;
			background-color: rgba(255,82,65,1);
			margin-right: 20upx;
		}
	}
	.grid{
		display: flex;
		align-items: center;
		flex-wrap: wrap;
		width: 95%;
		margin: 0 2.5%;
		.grid-item{
			box-shadow: 5upx 10upx 15upx rgba(#7a7a7a, 0.2);
			box-sizing: border-box;
			width: calc(100% / 2 - 40rpx);
			text-align: center;
			padding: 30rpx 0;
			position: relative;
			background-color: #ffffff;
			border-radius: 10rpx;
			margin: 0 20rpx;
			margin-top: 20rpx;
			image{
				width: 100rpx;
				height: 100rpx;
			}
			view{
				font-size: 28upx;
				margin: 10upx 0;
				color: rgba(0,0,0,.53);
			}
			text{
				display: block;
				padding: 8upx;
				text-align: center;
				border-radius: 50%;
				font-size: 24upx;
				background-color: rgba(255,82,65,1);
				color: rgba(255,255,255,1);
				position: absolute;
				right: 6upx;
				top: 6upx;
				width: 30upx;
				height: 30upx;
				line-height: 30upx;
			}
		}
	}
</style>
