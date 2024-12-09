<template>
  <view class="map-box">
    <map id="map" class="map" :latitude="latitude" :longitude="longitude" show-location :scale="10" :markers="markers"
         @markertap="handleMarkerTap" @tap="handleMapTap"/>
    <view v-show="isScan" class="scan-box scan-title" @tap="handleScan">扫码借还</view>
    <view v-show="!isScan" class="scan-box marker-sunmmary">
      <!-- left -->
      <view class="scan-box-left">
        <image src="/static/logo.png" mode="widthFix"></image>
        <!-- <image :src="currentMarker?.iconPath" mode="widthFix"></image> -->
      </view>
      <view class="scan-box-center">
        <view class="top scan-box-center-item">
          {{ currentMarker?.title || '-' }}
        </view>
        <view class="bottom scan-box-center-item">
          {{ currentMarker?.address || '-' }}
        </view>
      </view>
      <view class="scan-box-right" @tap="handleNavTap">
        <image src="/static/nav.png" mode="widthFix"></image>
      </view>
    </view>
    <view class="action-box">
      <!-- <image src="../../static/search.png" mode="widthFix"></image>
      <image src="../../static/support.png" mode="widthFix"></image>
      <image src="../../static/origin.png" mode="widthFix"></image>
      <image src="../../static/profile.png" mode="widthFix"></image> -->
      <image
          v-for="item in actionList"
          :key="item.id"
          :src="item.src"
          mode="widthFix"
          @tap="handleImageTap(item)"
      ></image>
    </view>
  </view>
</template>

<script setup>
import { onLoad } from '@dcloudio/uni-app';
import { ref } from 'vue';
import { markers } from '../../pages-data';

const longitude = ref(0);
const latitude = ref(0);
const currentMarker = ref(null);
const isScan = ref(true);

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

const handleMarkerTap = (e) => {
  console.log('handleMarkerTap', e.detail.markerId);
  const current = markers.find((i) => i.id === e.detail.markerId);
  console.log('current', current);
  currentMarker.value = current;
  isScan.value = Boolean(!current.id);
};

const handleMapTap = (e) => {
  console.log('handleMapTap');
  // isScan.value = true;
}

// 扫码
const handleScan = (e) => {
  uni.scanCode({
    success(res) {
      console.log('res', res.result.split('=')[ 1 ]);
    }
  })
}
// 跳转到场馆详情
const handleNavTap = () => {
  console.log('跳转到场馆详情');
  uni.navigateTo({
    url: `/pages/ground/ground?id=${ currentMarker.value.id }`
  });
};

const actionList = [
  { id: 1, src: '/static/search.png', name: '地图搜索' },
  { id: 2, src: '/static/support.png', name: '客服中心' },
  { id: 3, src: '/static/origin.png', name: '回到原点' },
  { id: 4, src: '/static/profile.png', name: '个人中心' }
];

const handleImageTap = (item) => {
  switch (item.id) {
    case 1:
      uni.chooseLocation({
        success(res) {
          console.log('res', res);
          longitude.value = res.longitude;
          latitude.value = res.latitude;
        }
      });
      break;
    case 2:
      console.log('客服中心');
      uni.navigateTo({
        url: '/pages/contact/contact'
      });
      break;
    case 3:
      // 和map的id对应
      const mapContext = uni.createMapContext('map');
      mapContext.moveToLocation();
      break;
    case 4:
      console.log('个人中心');
      uni.navigateTo({
        url: '/pages/profile/profile'
      });
      break;
    default:
      break;
  }
};

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
