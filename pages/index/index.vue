<template>
  <web-view :src="webUrl"></web-view>
</template>

<script setup>
import { onMounted, onUnmounted,ref } from 'vue'

// 网页地址
const webUrl = ref('https://rbxmaofu.maoaoll.asia')

// 返回键监听器
let backListener = null

// 处理返回逻辑
const handleBackAction = async (e) => {
  e.preventDefault() // 必须阻止默认行为
  
  try {
    const currentPage = plus.webview.currentWebview()
    if (currentPage.children().length > 0) {
      const wv = currentPage.children()[0]
      wv.canBack(res => {
        if (res.canBack) {
          wv.back()
        } else {
          uni.navigateBack()
        }
      })
    }
  } catch (error) {
    console.error('返回操作失败:', error)
    uni.navigateBack()
  }
}

// 初始化监听
onMounted(() => {
  // #ifdef APP-PLUS
  if (plus.key) {
    backListener = plus.key.addEventListener('backbutton', handleBackAction)
    console.log('返回键监听已注册')
  }
  // #endif
})

// 销毁监听
onUnmounted(() => {
  // #ifdef APP-PLUS
  if (backListener) {
    plus.key.removeEventListener(backListener)
    console.log('返回键监听已移除')
  }
  // #endif
})
</script>