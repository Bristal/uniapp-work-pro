<template>
	<view class="container">
		<view class="wrap">
			<view class="title">带小数点的金额键盘{{ amount1 }}</view>
			<AmountKeyboard v-model="amount1" :dotDisabled="false" border="surround" :decimalLimit="3" />
		</view>
		<view class="wrap">
			<view class="title">不带小数点的金额键盘{{ amount2 }}</view>
			<AmountKeyboard v-model="amount2" :dotDisabled="true" border="surround" />
		</view>
	</view>
</template>

<script setup>
import AmountKeyboard from '@/components/AmountKeyboard/AmountKeyboard.vue'
import { ref } from 'vue'

const amount1 = ref('')
const amount2 = ref('')
</script>

<style scoped lang="scss">
.container {
	min-height: 100vh;
	padding: 30rpx;
	background-color: #f5f5f5;
	box-sizing: border-box;
}

.wrap {
	background-color: #fff;
	border-radius: 16rpx;
	overflow: hidden;
	padding: 30rpx;
	margin: 20rpx 0;
}

.title {
	height: 100rpx;
	line-height: 100rpx;
	font-size: 32rpx;
}
</style>