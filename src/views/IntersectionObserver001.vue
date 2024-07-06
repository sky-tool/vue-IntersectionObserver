<template>
  <div>
    <div>
      注意观察 console 信息打印， 这里主要验证 当页面创建渲染之后再添加 IntersectionObserver
      监听，是否会生效，结果证明会生效
    </div>
    <button @click="toggleVisibility">Toggle Visibility</button>
    <div v-if="isVisible" ref="observedElement">This element is being observed</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isVisible: true,
      observer: null
    }
  },
  mounted() {
    this.setupObserver()
  },
  methods: {
    setupObserver() {
      this.observer = new IntersectionObserver((entries) => {
        entries.forEach((entry) => {
          console.log('Intersection:', entry.isIntersecting)
        })
      })

      if (this.$refs.observedElement) {
        this.observer.observe(this.$refs.observedElement)
      }
    },
    toggleVisibility() {
      this.isVisible = !this.isVisible
      // 重要：当元素重新显示时，我们需要重新设置观察者
      this.$nextTick(() => {
        if (this.isVisible) {
          setTimeout(() => {
            this.setupObserver()
          }, 3000)
        }
      })
    }
  },
  beforeUnmount() {
    if (this.observer) {
      this.observer.disconnect()
    }
  }
}
</script>
