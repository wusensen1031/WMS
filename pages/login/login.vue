<template>
	<view class="login">
		<view class="bg"></view>
		<uni-transition custom-class="transition" mode-class="fade" :show="!isRoot">
			<view>
				<view class="title" v-if="cTop > 0" :style="{top:cTop+'px'}">恒有WMS系统</view>
				<view class="content">
					<view class="main">
						<wInput
							v-model="phoneData"
							type="text"
							maxlength="11"
							placeholder="输入账号"
						></wInput>
						<wInput
							v-model="passData"
							type="password"
							maxlength="11"
							placeholder="输入密码"
							isShowPass
						></wInput>
					</view>
					<wButton2 
						class="wbutton"
						bgColor="#2896FF"
						text="登 录"
						:rotate="isRotate" 
						@click="startLogin"
					></wButton2>
					
					<wButton2
						bgColor="#aa0000"
						class="wbutton"
						text="重置服务地址"
						@click="reUrl"
					></wButton2>
				</view>
			</view>
		</uni-transition>
		<view v-show="isRoot">
			<prompt
				:isMutipleLine="false"
				:visible.sync="promptVisible"
				inputStyle=""
				title="配置服务地址"
				placeholder="例如:http://127.0.0.1:8080/"
				:defaultValue="defaultValue"
				@confirm="clickPromptConfirm"
			></prompt>
		</view>
		
		<view class="footer"><text>恒有智能云管理</text></view>
	</view>
</template>

<script>
	import wInput from '../../components/watch-login/watch-input.vue' 
	import wButton2 from '../../components/watch-login/watch-button-2.vue' 
	import Prompt from '@/components/zz-prompt/index.vue';
	export default {
		data() {
			return {
				isRoot:false,
				phoneData:'',
				passData:'',
				isRotate: false,
				cTop:0,
				
				promptVisible: true,
				defaultValue: 'http://127.0.0.1:8080/'
			};
		},
		components:{
			wInput,
			wButton2,
			Prompt
		},
		onLoad(e) {
			if (e.rootUrl) {
				this.defaultValue = e.rootUrl;
				let _this = this;
				setTimeout(function() {
					_this.getCTop();
				}, 300);
			}else{
				this.isRoot = true;
			}
		},
		onReady() {
			setTimeout(function() {
				plus.navigator.setStatusBarStyle('light');
			}, 150);
		},
		methods: {
		    startLogin(e){
				this.isRotate = true;
				var _thiss = this;
				setTimeout(function() {
					_thiss.isRotate = false;
					uni.redirectTo({
						url:'../index/index'
					})
				}, 2000);
				return ;
				
				if(this.isRotate){
					return false;
				}
				if (this.phoneData.length == "") {
				     uni.showToast({
				        icon: 'none',
						position: 'bottom',
				        title: '账号不能为空'
				    });
				    return;
				}
				if (this.passData.length == "") {
				     uni.showToast({
				        icon: 'none',
						position: 'bottom',
				        title: '密码不能为空'
				    });
				    return;
				}
				this.isRotate = true;
				let _this = this;
				var rootUrl = uni.getStorageSync('rootUrl');
				uni.request({
					url: rootUrl + '/zg-interface/app/appLoginByTeemlink.do',
					method:'POST',
					data:{
						phoneData:this.phoneData,
						passData:this.passData
					},
					success: (e) => {
						if(e.data.code == 1){
							this.isRotate = false;
							uni.setStorageSync("isLogin",true);
							uni.setStorageSync("user",e.data.data);
							uni.switchTab({
							    url: '/pages/index/index'
							});
						}else{
							uni.showToast({
								title:'账号或密码错误！',
								icon:'none',
								duration:2000,
								mask:true
							})
							setTimeout(function() {
								_this.isRotate = false;
							}, 2000);
						}
					},
					fail: (err) => {}
				})	
		    },
			reUrl(){
				if(this.isRotate){
					return false;
				}
				this.isRoot = true;
			},
			clickPromptConfirm(val) {
				if (val != '') {
					uni.setStorageSync('rootUrl', val);
					this.defaultValue = val;
					this.isRoot = false;
					
					let _this = this;
					setTimeout(function() {
						_this.getCTop();
					}, 300);
				}
			},
			getCTop(){
				let _this =this;
				uni.createSelectorQuery()
					.in(this)  
					.select(".content")  
					.boundingClientRect((data) => {
						uni.getSystemInfo({
						    success: function (res) {
						        _this.cTop = (res.screenHeight - data.bottom) / 2 ;
						    }
						});
						
					}).exec();  
			}
		}
	}
</script>

<style lang="scss">
	.login {
		width: 100vw;
		height: 100vh;
		display: flex;
		flex-direction: column;
		justify-content:center;
		background-color: #fff;
	}
	.title{
		font-size: 70rpx;
		color: #ffffff;
		font-weight: 700;
		padding-left: 70rpx;
		position: absolute;
		top: 0;
	}
	.content{
		position: relative;
		z-index: 100;
	}
	.main {
		display: flex;
		flex-direction: column;
		padding-left: 70rpx;
		padding-right: 70rpx;
	}
	.tips {
		color: #999999;
		font-size: 28rpx;
		margin-top: 64rpx;
		margin-left: 48rpx;
	}
	
	/* 登录按钮 */
	.wbutton{
		margin-top: 65rpx;
	}

	.footer{
		position: fixed;
		bottom: 50rpx;
		color: #ffffff90;
		width: 100vw;
		text-align: center;
	}
	.footer text{
		font-size: 28rpx;
		margin-left: 15rpx;
		margin-right: 15rpx;
	}
	
	.bg{
		width: 100vw;
		height: 100vh;
		position: absolute;
		top: 0;
		background-image: url(img/bg.gif);
		background-position: center;
		background-repeat: no-repeat;
		background-size: cover;
	}
</style>
