<template>
  <view class="container">
    <view class="nav-list">
      <view class="nav-item" v-for="item in list" @click="navigateTo(item.url)">
        <text class="nav-text">{{ item.name }}</text>
      </view>
    </view>
  </view>
</template>

<script setup>
import { ref } from 'vue'
const list = ref([{
  name: '金额键盘',
  url: '/views/keyboard/index'
}])

const navigateTo = (url) => {
  uni.navigateTo({
    url: url
  })
}
</script>

<style>
.container {
  min-height: 100vh;
  background-color: #f5f5f5;
  padding: 30rpx;
}

.header {
  padding: 60rpx 0;
  text-align: center;
}

.title {
  font-size: 48rpx;
  font-weight: bold;
  color: #333;
}

.nav-list {
  background-color: #fff;
  border-radius: 16rpx;
  overflow: hidden;
  box-shadow: 0 2rpx 10rpx rgba(0, 0, 0, 0.05);
}

.nav-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 30rpx 40rpx;
  border-bottom: 1rpx solid #f0f0f0;
  transition: background-color 0.2s;
}

.nav-item:last-child {
  border-bottom: none;
}

.nav-item:active {
  background-color: #f9f9f9;
}

.nav-text {
  font-size: 32rpx;
  color: #333;
}

.arrow {
  font-size: 28rpx;
  color: #ccc;
}
</style>
