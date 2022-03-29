<template>
	<view class="content">
		<prompt
			:isMutipleLine="false"
			:visible.sync="promptVisible"
			inputStyle=""
			title="配置服务地址"
			placeholder="例如:http://127.0.0.1:8080/"
			:defaultValue="defaultValue"
			@confirm="clickPromptConfirm"
		></prompt>
		<view class="bg"></view>
	</view>
</template>

<script>
import Prompt from '@/components/zz-prompt/index.vue';
export default {
	components: {
		Prompt
	},
	data() {
		return {
			promptVisible: true,
			defaultValue: 'http://127.0.0.1:8080/'
		};
	},
	onLoad(e) {
		if (e.rootUrl) {
			this.defaultValue = e.rootUrl;
		}
	},
	onReady() {
		setTimeout(function() {
			plus.navigator.setStatusBarStyle('light');
		}, 150);
	},
	methods: {
		clickPromptConfirm(val) {
			if (val != '') {
				uni.setStorageSync('rootUrl', val);
				uni.redirectTo({
					url: '/pages/guide/guide'
				});
			}
		}
	}
};
</script>

<style lang="scss">
.content {
	text-align: center;
	width: 100vw;
	height: 100vh;
	.bg{
		width: 100%;
		height: 100%;
		background-image: url(img/bg.jpg);
		background-position: center;
		background-repeat: no-repeat;
		background-size: cover;
		filter: blur(0px);
	}
}
</style>
