<script>
	export default {
		onLaunch: function() {
			uni.removeStorageSync("launchFlag");
			console.log('App Launch')
			/**
			 * 延迟关闭启动页
			 */
			// #ifdef  APP-PLUS
			setTimeout(function() {
				plus.navigator.closeSplashscreen();
			}, 1500);
			// #endif
			
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style lang="scss">
	@import "@/uni_modules/uview-ui/index.scss";
</style>
