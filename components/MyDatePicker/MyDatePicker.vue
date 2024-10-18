<template>
	<view class="container">
		<!-- 输入框 -->
		<view  class="input">
      <slot name="input">
				<input :placeholder="placeholder">
			</slot>
    </view>
		<!-- 日期面板 -->
		<DatePannel :isShow="isShow"></DatePannel>
		<!-- 遮罩层 -->
		<view class="mask"></view>
	</view>
</template>

<script>
	/**
	 * @description 仿elementui的日期选择器
	 * @property {Boolean} readonly 完全只读
	 * @property {Boolean} disabled 禁用
	 * @property {Boolean} editable 文本框可输入
	 * @property {Boolean} clearable 是否显示清除按钮
	 * @property {String} size 输入框尺寸，large, small, mini
	 * @property {String} placeholder 非范围选择时的占位内容
	 * @property {String} startPlaceholder 范围选择时开始日期的占位内容
	 * @property {String} endPlaceholder 范围选择时结束日期的占位内容
	 * @property {String} type 显示类型，year/month/date/dates/months/years week/datetime/datetimerange/daterange/monthrange
	 * @property {String} format 显示在输入框中的格式
	 * @property {String} align 对齐方式
	 * @property {String} popperClass DatePicker 下拉框的类名
	 * @property {Object} pickerOptions 当前时间日期选择器特有的选项
	 * @property {String} rangeSeparator 选择范围时的分隔符
	 * @property {Date} defaultValue 可选，选择器打开时默认显示的时间
	 * @property {String} defaultTime 范围选择时选中日期所使用的当日内具体时刻，数组，长度为 2，每项值为字符串，形如12:00:00，第一项指定开始日期的时刻，第二项指定结束日期的时刻，不指定会使用时刻 00:00:00
	 * @property {String} valueFormat 可选，绑定值的格式。不指定则绑定值为 Date 对象
	 * @property {String} name 原生属性
	 * @property {Boolean} unlinkPanels 在范围选择器里取消两个日期面板之间的联动
	 * @property {String} prefixIcon 自定义头部图标的类名
	 * @property {String} clearIcon 自定义清空图标的类名
	 * @property {Boolean} validateEvent 输入时是否触发表单的校验
	 * @property {Array} holidays 节假日
	 * @event {Function} change 用户确认选定的值时触发，组件绑定值。格式与绑定值一致，可受 value-format 控制
	 * @event {Function} blur 当 input 失去焦点时触发，组件实例
	 * @event {Function} focus 当 input 获得焦点时触发，组件实例
	 */
	import DatePannel from './DatePannel.vue'
	export default {
		name: "MyDatePicker",
		components: {
			DatePannel
		},
		props: {
			readonly: {
				type: Boolean,
				default: false
			},
			disabled: {
				type: Boolean,
				default: false
			},
			editable: {
				type: Boolean,
				default: true
			},
			clearable: {
				type: Boolean,
				default: true
			},
			size: {
				type: String,
				default: null
			},
			placeholder: {
				type: String,
				default: '请选择'
			},
			startPlaceholder: {
				type: String,
				default: '开始日期'
			},
			endPlaceholder: {
				type: String,
				default: '结束日期'
			},
			type: {
				type: String,
				default: 'date'
			},
			format: {
				type: String,
				default: 'yyyy-MM-dd'
			},
			align: {
				type: String,
				default: 'left'
			},
			popperClass: {
				type: String,
				default: null
			},
			pickerOptions: {
				type: Object,
				default: () => {}
			},
			rangeSeparator: {
				type: String,
				default: '-'
			},
			defaultValue: {
				type: Date,
				default: null
			},
			defaultTime: {
				type: String,
				default: null
			},
			valueFormat: {
				type: String,
			},
			name: {
				type: String,
				default: null
			},
			unlinkPanels: {
				type: Boolean,
				default: false
			},
			prefixIcon: {
				type: String,
				default: 'el-icon-date'
			},
			clearIcon: {
				type: String,
				default: 'el-icon-circle-close'
			},
			validateEvent: {
				type: Boolean,
				default: true
			},
			holidays: {
				type: Array,
				default: () => []
			}
			
		},
		data() {
			return {
				// 是否显示
				isShow: false
			};
		},
		methods: {
		  open() {
				this.isShow = true
			},
			close() {
				this.isShow = false
			}
		}
	}
</script>

<style lang="scss" scoped>
.container {
	position: fixed;
	left: 0;
	top: 0;
	width: 100%;
	height: 100%;

	.mask {
		position: fixed;
		left: 0;
		top: 0;
		right: 0;
		bottom: 0;
		z-index: -1;
		background: rgba(0, 0, 0, 0.5);
	}
}
</style>
