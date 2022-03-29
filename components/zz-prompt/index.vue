<template>
  <view class="prompt-box" v-if="visible" @click="hide">
    <view class="prompt" @click.stop="nothing">
      <view class="prompt-top">
        <text class="prompt-title">{{title}}</text>
        <input v-if="!isMutipleLine" class="prompt-input" :style="inputStyle" type="text" :placeholder="placeholder" v-model="value">
		<textarea v-else class="prompt-input" :style="inputStyle" type="text" :placeholder="placeholder" v-model="value"></textarea>
		<image @click="scanRootUrl" src="/static/login/scan.png"></image>
	  </view>
      <slot></slot>
      <view class="prompt-buttons">
        <button class="prompt-cancle" @click="clear">清空</button>
        <button class="prompt-confirm" @click="confirm">确定</button>
      </view>
    </view>
  </view>
</template>

<script>
	// 获取 module
	var ScanModule = uni.requireNativePlugin("wss-scan");
	export default {
    props: {
      visible: {
        type: Boolean,
        default: false,
        required: true,
      },
      title: {
        type: String,
        default: '提示',
      },
      placeholder: {
        type: String,
        default: '请输入内容',
      },
      mainColor: {
        type: String,
        default: '#e74a39',
      },
      defaultValue: {
        type: String,
        default: '',
      },
      inputStyle: {
        type: String,
        default: '',
      },
      // 是否多行输入的textarea
      isMutipleLine: {
        type: Boolean,
        default: false,
      }
    },
    data() {
      return {
        value: '',
		scanObj:{
			"scanMode":"default",
			"scanKey":"scanURL",
		},
      }
    },
    watch: {
      visible(val) {
        if(val) {
          this.value = this.defaultValue;
        }
      }
    },
    mounted() {
      this.value = this.defaultValue === 'true' ? '' : this.defaultValue;
    },
    methods: {
	  hide(){
		  this.$emit('confirm', '');
		  this.value = ''
	  },
	  nothing(){
		  
	  },
      clear() {
		this.value = ''
      },
      confirm() {
        this.$emit('confirm', this.value)
        //this.value = ''
      },
	  scanRootUrl(){
			ScanModule.scan(this.scanObj,
				(result) => {
					var ret = result.scanValue;
					this.value = ret.sValue;
		    })
	  }
    }
  }
</script>

<style scoped lang="scss">
  view,
  button,
  input {
    box-sizing: border-box;
  }

  .prompt-box {
    position: fixed;
    left: 0;
    top: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 100vh;
    background: rgba(0, 0, 0, .2);
    transition: opacity .2s linear;
	z-index: 1000;
  }

  .prompt {
    position: relative;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
    width: 600upx;
    min-height: 300upx;
    background-color: rgba($color: #ffffff, $alpha: 1);
	opacity:0.6;
    border-radius: 20upx;
    overflow: hidden;
  }

  .prompt-top {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 100%;
  }

  .prompt-title {
    margin: 20upx 0;
    color: #333;
  }

  .prompt-input {
    width: 520upx;
    min-height: 72upx;
    padding: 8upx 16upx;
    border: 2upx solid #ddd;
    border-radius: 8upx;
    font-size: 28upx;
    text-align: left;
	padding-right: 80upx;
  }

  .prompt-buttons {
    display: flex;
    width: 100%;
    box-shadow: 0 0 2upx 2upx #eee;
  }
  
  .prompt-buttons button:after {
    border-radius: 0;
  }
  
  button {
    width: 50%;
    background: white;
    border-radius: 0;
	line-height: 85upx;
  }

  .prompt-cancle {
    background: white;
	color: $uni-color-app;
  }

  .prompt-confirm {
    color: white;
	background-color: $uni-color-app;
  }
  
  image{
	  width: 62upx;
	  height: 62upx;
	  margin-top: -67upx;
	  margin-left: 445upx;
  }
</style>
