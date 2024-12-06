<template>
  <view class="map-box">
    <map class="map" :latitude="latitude" :longitude="longitude" show-location :scale="10" />
  </view>
</template>

<script setup>
import { onLoad } from '@dcloudio/uni-app';
import { ref } from 'vue';

const longitude = ref(0);
const latitude = ref(0);
onLoad(() => {
  uni.getLocation({
    success(res) {
      console.log('res', res);
      latitude.value = res.latitude;
      longitude.value = res.longitude;
    },
    fail(err) {
      console.log('err', err);
    }
  });
});
</script>

<style>
.map-box {
  position: relative;
}

.map {
  width: 100vw;
  height: 100vh;
}

.scan-box {
  position: absolute;
  width: 80%;
  height: 120rpx;
  left: 10%;
  bottom: 100rpx;
  background-color: #1296db;
  box-shadow: 0 0 20rpx #1296db;
  border-radius: 20rpx;
  padding: 20rpx 0;
  display: flex;
}

.scan-title {
  font-size: 50rpx;
  font-weight: bold;
  justify-content: center;
  align-items: center;
  color: white;
  letter-spacing: 4rpx;
}

.marker-sunmmary {
  background-color: white;
}

.scan-box-left,
.scan-box-right {
  width: 120rpx;
  /* background-color: #1296db; */
  margin: 0 20rpx;
}

.scan-box-left image,
.scan-box-right image {
  width: 100%;
  border-radius: 50%;
}

.scan-box-right image {
  width: 80%;
  margin: 10% 0 0 10%;
}

.scan-box-center {
  flex: 1;
}

.scan-box-center-item {
  height: 60rpx;
  line-height: 60rpx;
  width: 280rpx;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.top {
  font-weight: bold;
}

.bottom {
  font-size: 28rpx;
}

.action-box {
  position: absolute;
  right: 10%;
  bottom: 300rpx;
  width: 90rpx;
  /* background-color: white; */
}

.action-box image {
  width: 50rpx;
  padding: 20rpx;
  border-radius: 50%;
  box-shadow: 0 0 20rpx #1296db;
  background-color: white;
  margin-bottom: 40rpx;
}
</style>
