<script setup>
import { onLoad } from '@dcloudio/uni-app';
import { ref } from 'vue';
import { getContactQuestionByType } from '../../pages-data';

const list = ref([]);
onLoad((options) => {
  // console.log('options', options);

  // const type = 1;
  // const res = getContactQuestionByType(type);
  // console.log('res', res);
  // 获取路径对应的参数
  if (options.type) {
    list.value = getContactQuestionByType(+options.type);
  }
});
</script>

<template>
  <view class="question-box">
    <view v-for="(item, index) in list" :key="item.id">
      <view class="question-title">{{ index + 1 }}. {{ item.title }}</view>
      <view class="question-desc">
        {{ item.desc }}
      </view>
    </view>
    <!-- <view class="question-title">1. 哈喽哈喽哈喽</view>
    <view class="question-desc">
      哈喽哈喽哈喽哈喽哈喽哈喽哈喽哈喽哈喽哈喽哈喽哈喽哈喽哈喽哈喽
    </view>
    <view class="question-title">1. 哈喽哈喽哈喽</view>
    <view class="question-desc">
      哈喽哈喽哈喽哈喽哈喽哈喽哈喽哈喽哈喽哈喽哈喽哈喽哈喽哈喽哈喽
    </view>
    <view class="question-title">1. 哈喽哈喽哈喽</view>
    <view class="question-desc">
      哈喽哈喽哈喽哈喽哈喽哈喽哈喽哈喽哈喽哈喽哈喽哈喽哈喽哈喽哈喽
    </view> -->
  </view>
</template>

<style>
page {
  background-color: #f1f1f1;
}
.question-box {
  width: 80%;
  margin: 0 auto;
  background-color: white;
  padding: 20rpx 5%;
}

.question-title {
  height: 100rpx;
  line-height: 100rpx;
  font-weight: bold;
  font-size: 40rpx;
}

.question-desc {
  font-size: 30rpx;
}
</style>
