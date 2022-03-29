<template>
	<view class="wap">
		<view class="status_bar">
		    <!-- 这里是状态栏占位 -->
		</view>
		<view class="content">
			<view class="headbg" :style="{height:headbgHeight+'rpx'}">
				<image src="/static/index/bl.png" class="img1"></image>
				<image src="/static/index/bl.png" class="img2"></image>
			</view>
			<view class="search" :class="(scrollOpacity >0 && scrollOpacity <1 )?'searchFixed':''" >
				<u-icon @click="kefu" name="kefu-ermai" size="35" color="#ffffff" class="kefu"></u-icon>
				<u-search class="inputSer" :disabled="true" :showAction="false" :clearabled="false" borderColor="#ffffff"></u-search>
				<u-icon  name="scan" size="40" color="#ffffff" class="scan"></u-icon>
			</view>
			<view class="scrollmask" :class="scrollOpacity >=0.1 ?'scrollmaskFixed':''" 
				:style="{opacity:scrollOpacity}">
			</view>
			<view class="workbench">
				<zy-grid title="入库" :grid-list="gridList1" :show-tip="true" :col="2"></zy-grid>
				<zy-grid title="出库" :grid-list="gridList2" :show-tip="true" :col="2"></zy-grid>
				<zy-grid title="自检" :grid-list="gridList3" :show-tip="true" :col="2"></zy-grid>
				<zy-grid title="功能" :grid-list="gridList4" :show-tip="true" :col="2"></zy-grid>
				<view class="bg"></view>
			</view>
		</view>
	</view>
</template>

<script>
	import zyGrid from '@/components/zy-grid/zy-grid.vue';
	import checkUpdate from "@/uni_modules/uni-upgrade-center-app/utils/check-update.js";
	export default {
		components: { zyGrid },
		data() {
			return {
				scrollOpacity:0,
				headbgHeight:200,
				gridList1: [
					{ name: '成品入库', imgUrl: '/static/index/warehousing.png', tips: '', path: '/pages/form/warehousing/warehousing_order',click:false },
					{ name: '车间入库', imgUrl: '/static/index/halfwarehousing.png', tips: '', path: '/pages/form/halfwarehousing/warehousing_order',click:false },
				],
				gridList2: [
					{ name: '扫码出库', imgUrl: '/static/index/unwarehousing.png', tips: '', path: '/pages/form/unwarehousing/unwarehousing_order',click:false },
				],
				gridList3: [
					{ name: '扫码检验', imgUrl: '/static/index/operateBarcode.png', tips: '', path: '/pages/form/barcode/operateBarcode',click:false },
					{ name: '检验清单', imgUrl: '/static/index/listBarcode.png', tips: '', path: '/pages/form/barcode/listBarcode',click:false },
				],
				gridList4: [
					{ name: '条码查询', imgUrl: '/static/index/searchBarcode.png', tips: '', path: '/pages/form/barcode/searchBarcode',click:false },
				]
			}
		},
		onLoad() {
			checkUpdate();
			let _this = this;
			if(plus.navigator.hasNotchInScreen()){//判断是否是刘海屏
				_this.headbgHeight = 250;
			}
			uni.getSystemInfo({
			    success: function (res) {
			        _this.statusBarHeight = res.statusBarHeight;
			    }
			});
		},
		onReady() {
			let _this = this;
			uni.createSelectorQuery().in(this).select(".search").boundingClientRect((data) => {
					_this.searchBottom = data.bottom;
				}).exec();
			setTimeout(function() {
				plus.navigator.setStatusBarStyle('light');
			}, 150);
		},
		onPageScroll(e){
			let scroll = e.scrollTop;
			let maskHeight = this.statusBarHeight + 40;
			this.scrollOpacity = scroll / maskHeight;
		},
		methods: {
			kefu(){
				uni.navigateTo({
					url:'../kefu/kefu'
				})
			}
		}
	}
</script>

<style lang="scss">
	page{
		background-color: #efefef;
	}
	.status_bar {
	      height: var(--status-bar-height);
	      width: 100%;
		  position: fixed;
		  top: 0;
		  z-index: 100;
	  }
	  .content{
		  position: relative;
		  .headbg{
			  height: 200rpx;
			  background-color: $uni-color-app;
			  position: relative;
			  image{
				  width: 100vw;
				  position: absolute;
				  bottom: 0;
				  opacity: 0.2;
			  }
			  .img1{
				  height: 100rpx;
				  left: -30vw;
				  width: 130vw;
			  }
			  .img2{
				  height: 60rpx;
			  }
		  }
		  .data{
			  width: 90%;
			  margin: 0 5%;
			  position: relative;
			  z-index: 10;
			  background-color: #ffffff;
			  border-radius: 10rpx;
			  margin-top: calc(-250rpx + 40px + var(--status-bar-height) + 10px);
			  box-shadow: 5upx 10upx 15upx rgba(#7a7a7a, 0.2);
			  .line{
				  width: 1%;
				  display: inline-block;
				  position: absolute;
				  top: 50%;
				  transform: translateY(-50%);
			  }
			  .item{
				  width: 49%;
				  padding: 20rpx 0;
				  display: inline-block;
				  text-align: center;
				  .content{
						.img{
							position: relative;
							left: 50%;
							transform: translateX(-50%);
							width: 80rpx;
							height: 80rpx;
							image{
								width: 100%;
								height: 100%;
							}
						}
						.num{
							margin-top: 10rpx;
							font-size: 44rpx;
							color: #424242;
						}
						.text{
							font-size: 28rpx;
							color: #838383;
						}
					}
			  }
		  }
		  .scrollmask{
			  position: fixed;
			  top: 0;
			  height: calc(var(--status-bar-height) + 45px);
			  width: 100%;
			  background-color: #ffffff;
			  z-index: 100;
		  }
		  .scrollmaskFixed{
			  background-color: $uni-color-app;
			  opacity: 0.1;
		  }
		  .search{
			  position: fixed;
			  top:  var(--status-bar-height);
			  width: 90%;
			  margin: 0 5%;
			  z-index: 101;
			  .kefu{
				  float: left;
				  margin-right: 15rpx;
				  margin-top: -5rpx;
			  }
			  .inputSer{
				   float: left;
				   margin-right: 20rpx;
				   width: calc(100% - 40px - 40px);
			  }
			  .scan{
				  margin-top: -10rpx;
			  }
		  }
		  .searchFixed{
			  opacity: 0.6;
		  }
		  .workbench{
			  width: 100%;
			  position: absolute;
			  margin-bottom: 40rpx;
			  padding-bottom: 40rpx;
			  .bg{
				  position: fixed;
				  top: 40rpx;
				  width: 100vw;
				  height: 100%;
				  background-image: url(../../static/index/bg.png);
				  background-position: center;
				  background-repeat: no-repeat;
				  background-size: cover;
				  z-index: -1;
				  opacity: 0.5;
			  }
		  }
	  }
</style>
