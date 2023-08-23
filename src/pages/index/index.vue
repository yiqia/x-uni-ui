<template>
  <view class="content">
    <image class="logo" src="/static/logo.png" />
    <view class="text-area">
      <text class="title">{{ title }}</text>
    </view>
    <x-button>sss</x-button>
    <x-city-picker :value="pickerValue" @change="onCityChange"></x-city-picker>
  </view>
</template>

<script setup lang="ts">
import { ref } from "vue";
const pickerValue = ref<number[]>([2, 2, 2]);
const cityPicker = () => {
  pickerValue.value = [5, 5, 5];
};
const onCityChange = (e: any) => {
  console.log("onCityChange", e);
};

const title = ref("Hello");
</script>

<style>
.content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.logo {
  height: 200rpx;
  width: 200rpx;
  margin-top: 200rpx;
  margin-left: auto;
  margin-right: auto;
  margin-bottom: 50rpx;
}

.text-area {
  display: flex;
  justify-content: center;
}

.title {
  font-size: 36rpx;
  color: #8f8f94;
}
</style>
