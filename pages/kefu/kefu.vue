<template>
	<view class="wap">
		<view class="service">
			<view class="title">客服咨询</view>
			<view class="text">
				<text>
					如果您在试用过程中有遇到问题或者优化意见，
					可点击下方【电话】拨打我司全国服务热线，
					也可与【微信客服】在线联系！
				</text>
			</view>
			<view class="line">
				<u-line dashed></u-line>
			</view>
			<view class="mode">
				<view class="item">
					<view class="img" @click="call"><image src="/static/kefu/phone.png"></image></view>
					<view class="text"><text>电话</text></view>
				</view>
				<view class="item">
					<view class="img"><image src="/static/kefu/weixin.png"></image></view>
					<view class="text"><text>微信客服</text></view>
				</view>
			</view>
			<view class="time">工作日9:00-18:00</view>
		</view>
		<view class="info">
			<view class="title">联系我们</view>
			<view class="text"><text class="t1">公司官网</text><text class="t2">www.hengyou.com</text></view>
			<view class="line"><u-line dashed></u-line></view>
			<view class="text"><text class="t1">微信公众号</text><text class="t2">hengyousoft</text></view>
			<view class="line"><u-line dashed></u-line></view>
			<view class="text"><text class="t1">服务热线</text><text class="t2">15306605860</text></view>
		</view>
	</view>
</template>

<script>
	export default {
		components: { },
		data() {
			return {
			}
		},
		methods: {
			call(){
				uni.makePhoneCall({
				    phoneNumber: '15306605860'
				});
			}
		}
	}
</script>

<style lang="scss">
	page{
		background-color: #efefef;
	}
	.wap{
		width: 100vw;
		.service{
			width: 95%;
			margin: 0 2.5%;
			background-color: #ffffff;
			padding-bottom: 20rpx;
			.title{
				color: #4f4f4f;
				font-weight: 700;
				font-size: 34rpx;
				margin-top: 10rpx;
				width: 95%;
				margin: 10rpx 2.5%;
				margin-top: 20rpx;
				padding-top: 20rpx;
			}
			.text{
				color: #535353;
				font-size: 30rpx;
				width: 90%;
				margin: 10rpx 5%;
				margin-top: 30rpx;
			}
			.line{
				width: 90%;
				margin: 0 5%;
				margin-top: 20rpx;
			}
			.mode{
				width: 100%;
				display: flex;
				margin-top: 20rpx;
				.item{
					flex: 1;
					width: 50%;
					text-align: center;
					.img{
						display: inline-block;
						width: 50rpx;
						height: 50rpx;
						image{
							width: 100%;
							height: 100%;
						}
					}
					.text{
						position: relative;
						font-size: 28rpx;
						color: #717171;
						top: -40rpx;
					}
				}
			}
			.time{
				width: 100%;
				text-align: center;
				color: #717171;
				font-size: 28rpx;
			}
		}
		.info{
			width: 95%;
			margin: 0 2.5%;
			background-color: #ffffff;
			padding-bottom: 20rpx;
			.title{
				color: #4f4f4f;
				font-weight: 700;
				font-size: 34rpx;
				margin-top: 10rpx;
				width: 95%;
				margin: 10rpx 2.5%;
				margin-top: 20rpx;
				padding-top: 20rpx;
			}
			.text{
				color: #535353;
				font-size: 30rpx;
				width: 90%;
				margin: 10rpx 5%;
				margin-top: 30rpx;
				.t2{
					float: right;
				}
			}
			.line{
				width: 90%;
				margin: 20rpx 5%;
				
			}
		}
	}
</style>
