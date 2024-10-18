<template>
  <view>
    <lotusCalendar :calendarData="calendarData" @returnDate="calendarChange" @closeCalendar="closeCalendar"></lotusCalendar>
  </view>
</template>

<script>
import lotusCalendar from "../../components/Winglau14-lotusCalendar/Winglau14-lotusCalendar.vue";

export default {
  components: {
    lotusCalendar,
  },
  data() {
    return {
      calendarData: {
        isShow: true,
        choseTime: '2022-03-26',
      },
      form: {
        businessDate: '',
      },
    }
  },
  methods: {
    // 日期选择
    calendarChange(res) {
      if (res.time) {
        this.form.businessDate = res.time
      }
      this.calendarData.isShow = res.isShow
    },
    // 关闭日期控件
    closeCalendar(res) {
      if (res.time) {
        this.form.businessDate = res.time
      }
      this.calendarData.isShow = res.isShow
    },
  },
}
</script>

<style></style>
