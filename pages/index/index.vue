<template>
  <view class="content">
    <!-- 默认控件 -->
    <MyDatePickerVue v-model="value1" type="date" placeholder="选择日期"></MyDatePickerVue>
    <!-- 带快捷选项 -->
    <!-- <MyDatePickerVue v-model="value2" align="right" type="date" placeholder="选择日期" :picker-options="pickerOptions0"> </MyDatePickerVue> -->
    <!-- 其他日期单位 -->
    <!-- 周 -->
    <!-- <MyDatePickerVue v-model="value3" type="week" format="yyyy 第 WW 周" placeholder="选择周"></MyDatePickerVue> -->
    <!-- 月 -->
    <!-- <MyDatePickerVue v-model="value4" type="month" placeholder="选择月"></MyDatePickerVue> -->
    <!-- 年 -->
    <!-- <MyDatePickerVue v-model="value5" type="year" placeholder="选择年"></MyDatePickerVue> -->
    <!-- 多个日期 -->
    <!-- <MyDatePickerVue v-model="value6" type="dates" placeholder="选择一个或多个日期"></MyDatePickerVue> -->
    <!-- 多个月 -->
    <!-- <MyDatePickerVue v-model="value7" type="months" placeholder="选择一个或多个月"></MyDatePickerVue> -->
    <!-- 多个年 -->
    <!-- <MyDatePickerVue v-model="value8" type="years" placeholder="选择一个或多个年"></MyDatePickerVue> -->
    <!-- 选择日期范围 -->
    <!-- <MyDatePickerVue v-model="value9" type="daterange" range-separator="至" start-placeholder="开始日期" end-placeholder="结束日期"></MyDatePickerVue> -->
    <!-- 选择日期范围，带快捷选项 -->
    <!-- <MyDatePickerVue
      v-model="value10"
      type="daterange"
      lign="right"
      unlink-panels
      range-separator="至"
      start-placeholder="开始日期"
      end-placeholder="结束日期"
      :picker-options="pickerOptions1"
    ></MyDatePickerVue> -->
    <!-- 选择月份范围 -->
    <!-- <MyDatePickerVue v-model="value11" type="monthrange" range-separator="至" start-placeholder="开始月份" end-placeholder="结束月份"></MyDatePickerVue> -->
    <!-- 选择月份范围，带快捷选项 -->
    <!-- <MyDatePickerVue
      v-model="value12"
      type="monthrange"
      align="right"
      unlink-panels
      range-separator="至"
      start-placeholder="开始月份"
      end-placeholder="结束月份"
      :picker-options="pickerOptions2"
    ></MyDatePickerVue> -->
  </view>
</template>

<script>
import MyDatePickerVue from '@/components/MyDatePicker/MyDatePicker.vue'
export default {
	components: {
		MyDatePickerVue
	},
	data() {
		return {
			value1: '',
			value2: '',
			value3: '',
			value4: '',
			value5: '',
			value6: '',
			value7: '',
			value8: '',
			value9: '',
			value10: '',
			value11: '',
			value12: '',
			pickerOptions0: {
				disabledDate(time) {
					return time.getTime() > Date.now();
				},
				shortcuts: [{
					text: '今天',
					onClick(picker) {
						picker.$emit('pick', new Date());
					}
				}, {
					text: '昨天',
					onClick(picker) {
						const date = new Date();
						date.setTime(date.getTime() - 3600 * 1000 * 24);
						picker.$emit('pick', date);
					}
				}, {
					text: '一周前',
					onClick(picker) {
						const date = new Date();
						date.setTime(date.getTime() - 3600 * 1000 * 24 * 7);
						picker.$emit('pick', date);
					}
				}]
			},
			pickerOptions1: {
				shortcuts: [{
					text: '最近一周',
					onClick(picker) {
						const end = new Date();
						const start = new Date();
						start.setTime(start.getTime() - 3600 * 1000 * 24 * 7);
						picker.$emit('pick', [start, end]);
					}
				}, {
					text: '最近一个月',
					onClick(picker) {
						const end = new Date();
						const start = new Date();
						start.setTime(start.getTime() - 3600 * 1000 * 24 * 30);
						picker.$emit('pick', [start, end]);
					}
				}, {
					text: '最近三个月',
					onClick(picker) {
						const end = new Date();
						const start = new Date();
						start.setTime(start.getTime() - 3600 * 1000 * 24 * 90);
						picker.$emit('pick', [start, end]);
					}
				}]
			},
			pickerOptions2: {
				shortcuts: [{
					text: '本月',
					onClick(picker) {
						picker.$emit('pick', [new Date(), new Date()]);
					}
				}, {
					text: '今年至今',
					onClick(picker) {
						const end = new Date();
						const start = new Date(new Date().getFullYear(), 0);
						picker.$emit('pick', [start, end]);
					}
				}, {
					text: '最近六个月',
					onClick(picker) {
						const end = new Date();
						const start = new Date();
						start.setMonth(start.getMonth() - 6);
						picker.$emit('pick', [start, end]);
					}
				}]
			}
		}
	},
	onLoad() {

	},
	methods: {

	}
}
</script>

<style>
.content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
</style>
