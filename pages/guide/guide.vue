<template>
    <view class="content"></view>
</template>

<script>

export default {
    onLoad() {
		var _this = this;
		_this.checkLogin();
    },
    methods: {
        checkLogin() {
			const launchFlag = uni.getStorageSync('launchFlag');
			//console.log(launchFlag)
			if (launchFlag) {
			    var isLogin = uni.getStorageSync('isLogin');
			    if (isLogin) {
			    	uni.switchTab({
			    	    url: '/pages/index/index'
			    	});
			    } else {
			    	var rootUrl = uni.getStorageSync('rootUrl');
			    	if(!rootUrl){
			    		uni.redirectTo({
			    		    url: '/pages/login/login'
			    		});
			    	}else{
			    		uni.redirectTo({
			    		    url: '/pages/login/login?rootUrl='+rootUrl
			    		});
			    	}
			    }
			}else{
				uni.redirectTo({
				    url: '/pages/guide/list'
				});
			}
        }
    }
};
</script>

<style></style>
