<template>
  <view :class="['pannel', isShow ? 'animationIn' : 'animationOut']">

  </view>
</template>

<script>
export default {
  props: {
    isShow: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      weeks: ['日', '一', '二', '三', '四', '五', '六'],
      // =============================== 第一个面板开始 ===============================
      // 第一个面板的日期
      days0: [],
      // =============================== 第一个面板结束 ===============================
      // =============================== 第二个面板开始 ===============================
      // 第二个面板的日期
      days1: [],
      // =============================== 第二个面板结束 ===============================
    };
  },
  methods: {
    methods: {
      // 循环获取天数
    }
  },
};
</script>

<style lang="scss" scoped>
.pannel {
  position: absolute;
  bottom: 0;
  transform: translateY(100%);
  background: #fff;
}

.animationIn {
  animation: animationIn 0.4s ease-in-out forwards;
}
.animationOut {
  animation: animationOut 0.4s ease-in-out reverse forwards;
}

@keyframes animationIn {
  0% {
    transform: translateY(100%);
  }
  100% {
    transform: translateY(0%);
  }
}
@keyframes animationOut {
  0% {
    transform: translateY(0%);
  }
  100% {
    transform: translateY(100%);
  }
}

</style>
