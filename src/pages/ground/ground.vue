<template>
  <view class="ground-box">
    <image
        class="ground-image"
        src="https://img.qtx.com/asset/ec/7a/63/0a3f99a3392ea8e3cb7a9d151bf92928.jpeg"
        mode="widthFix"
    ></image>
    <view class="ground-title">{{ groundInfo?.title }}</view>
    <view class="ground-detail">
      <view class="ground-detail-left">
        <image src="/static/location.png" mode="widthFix"></image>
      </view>
      <view class="ground-detail-center">{{ groundInfo?.address }}</view>
      <view class="ground-detail-right" @tap="handleTap">
        <image src="../../static/navTo.png" mode="widthFix"></image>
      </view>
    </view>
    <!-- 场馆热度 -->
    <view class="ground-detail">
      <view class="ground-detail-left">
        <image src="/static/hot.png" mode="widthFix"></image>
      </view>
      <view class="ground-detail-center">
        <text>场馆热度：{{ groundInfo?.hot }}</text>
        <text v-if="groundInfo?.hot === 'SSS'">（非常高）</text>
      </view>
    </view>
    <!-- 营业时间 -->
    <view class="ground-detail">
      <view class="ground-detail-left">
        <image src="/static/time.png" mode="widthFix"></image>
      </view>
      <view class="ground-detail-center">
        <text>
          营业时间：周一到周日 {{ groundInfo?.startTime }} - {{ groundInfo?.endTime }}
        </text>
      </view>
    </view>
    <!-- 停车场 -->
    <view class="ground-detail">
      <view class="ground-detail-left">
        <image src="/static/parking.png" mode="widthFix"></image>
      </view>
      <view class="ground-detail-center">
        <text>停车场： {{ groundInfo?.isPark ? '有' : '无' }}</text>
      </view>
    </view>
    <!-- 卫生间 -->
    <view class="ground-detail">
      <view class="ground-detail-left">
        <image src="/static/toilet.png" mode="widthFix"></image>
      </view>
      <view class="ground-detail-center">
        <text>卫生间： {{ groundInfo?.isToilet ? '有' : '无' }}</text>
      </view>
    </view>
    <!-- 洗浴室 -->
    <view class="ground-detail">
      <view class="ground-detail-left">
        <image src="/static/bathroom.png" mode="widthFix"></image>
      </view>
      <view class="ground-detail-center">
        <text>洗浴室： {{ groundInfo?.isBathroom ? '有' : '无' }}</text>
      </view>
    </view>
    <!-- 夜间灯光 -->
    <view class="ground-detail">
      <view class="ground-detail-left">
        <image src="/static/light.png" mode="widthFix"></image>
      </view>
      <view class="ground-detail-center">
        <text>夜间灯光： {{ groundInfo?.isLight ? '有' : '无' }}</text>
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
import {onLoad} from '@dcloudio/uni-app';
import {ref} from 'vue';
import {getGroundInfoById} from '../../pages-data'

const name = ref('');
const groundInfo = ref(null)
onLoad((options) => {
  console.log(options);
  const id = Number(options.id)
  if (id) {
    groundInfo.value = getGroundInfoById(id)
  }
})
const handleTap = () => {
  console.log(groundInfo.value, 'groundInfo');
  uni.openLocation({
    longitude: groundInfo.value.marker.longitude,
    latitude: groundInfo.value.marker.latitude,
    name: groundInfo.value.marker.title,
    address: groundInfo.value.marker.address,
    success(res) {
      console.log('res', res);
    },
    fail(err) {
      console.log('err', err);
    }
  });
};
</script>


<style scoped>
.ground-box {
  width: 90%;
  margin: 0 auto;
}

.ground-box .ground-image {
  width: 100%;
  border-radius: 20rpx;
  margin-top: 20rpx;
}

.ground-title {
  height: 80rpx;
  line-height: 80rpx;
  font-size: 30rpx;
  font-weight: bold;
}

.ground-detail {
  height: 80rpx;
  width: 100%;
  display: flex;
  border-bottom: 1rpx solid #1296db;
  box-sizing: border-box;
  margin-bottom: 20rpx;
}

.ground-detail-left,
.ground-detail-right {
  width: 80rpx;
  height: 80rpx;
  /* background-color: yellow; */
}

.ground-detail-left image,
.ground-detail-right image {
  width: 60rpx;
  margin: 10rpx 0;
}

.ground-detail-right {
  text-align: right;
}

.ground-detail-center {
  flex: 1;
  line-height: 80rpx;
  font-size: 30rpx;
  color: gray;
}
</style>
