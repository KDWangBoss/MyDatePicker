<!--
*calendarData.isShow:日历组件显示隐藏
*calendarData.yearStyle:设置年样式
*calendarData.monthStyle:设置月样式
*calendarData.dayStyle:设置日样式
*returnDate:已选日期回调方法，返回结果{time:'已选日期',timeType:'对应选择模块',isShow:'为了处理组件显示隐藏回调uniapp的坑'}
:status="checkStatus"
-->
<template>
  <view :status="checkStatus" v-if="calendarData.isShow" class="lotus-calendar-wrap">
    <view :class="calendarData.isShow ? 'lotus-calendar' : 'lotus-calendar lotus-calendar-out'">
      <view class="lotus-calendar-title">选择日期</view>
      <view class="lotus-calendar-cur-bg-month">{{ curMonth + 1 }}</view>
      <view class="lotus-calendar-cur-date">
        <view class="lotus-calendar-center">
          <view class="lotus-calendar-month">
            <view class="lotus-calendar-prev" @tap="clickPrevMonth">←</view>
            <view :style="calendarData.monthStyle" @tap="setStauts('showMonthFlag', true)" class="lotus-calendar-cur-text">{{ showCurMonth }}月</view>
            <view class="lotus-calendar-next" @tap="clickNextMonth">→</view>
          </view>
          <view class="lotus-calendar-year">
            <view class="lotus-calendar-prev" @tap="clickPrevYear">←</view>
            <view :style="calendarData.yearStyle" @tap="setStauts('showYearFlag', true)" class="lotus-calendar-cur-text">{{ curYear }}</view>
            <view class="lotus-calendar-next" @tap="clickNextYear">→</view>
          </view>
        </view>
      </view>
      <view class="lotus-calendar-week">
        <view class="lotus-calendar-week-text lotus-calendar-week-range" v-for="(item, index) in weekText" :key="index">
          {{ item }}
        </view>
      </view>
      <view class="lotus-calendar-days">
        <view class="lotus-calendar-days-text" v-for="(item, index) in totalDaysArr" :key="index" @tap="clickTargetTime(item, index)">
          <view :class="item.flag ? 'lotus-calendar-days-act' : 'lotus-calendar-days-gray'" v-if="item.type == 0">{{ item.day }}</view>
          <view :class="item.flag ? 'lotus-calendar-days-act' : ''" v-if="item.type == 1">{{ item.day }}</view>
          <view :style="calendarData.dayStyle" :class="item.flag ? 'lotus-calendar-days-act' : ''" v-if="item.type == 2">{{ item.day }}</view>
        </view>
      </view>
      <view class="lotus-calendar-result">
        <view class="lotus-calendar-result-time" v-text="selectTime"></view>
        <view class="lotus-calendar-result-btn" @tap="confirmFn">确定</view>
      </view>
    </view>
    <!--月份选择-->
    <view v-if="showMonthFlag" class="lotus-calendar-months">
      <!-- <view class="lotus-calendar-months-cancel">
				<image @tap="setStauts('showMonthFlag',false);" class="lotus-calendar-months-cancel-icon" src="data:image/png;base64,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" mode="aspectFit"></image>
			</view> -->
      <view class="lotus-calendar-months-box">
        <view v-for="(item, index) in monthArray" :key="index" :style="item.flag && calendarData.monthStyle" class="lotus-calendar-months-text">
          <text @tap="setAct(item, 'month')" v-text="item.monthText"></text>
        </view>
      </view>
    </view>
    <!--年份份选择-->
    <view v-if="showYearFlag" class="lotus-calendar-months">
      <view class="lotus-calendar-year lotus-calendar-year2">
        <view class="lotus-calendar-prev" @tap="yearRangeChanage(-10)">←</view>
        <view :style="calendarData.yearStyle" @tap="setStauts('showYearFlag', true)" class="lotus-calendar-cur-text">{{ curYear - 10 }}-{{ curYear }}</view>
        <view class="lotus-calendar-next" @tap="yearRangeChanage(+10)">→</view>
        <!-- <view class="lotus-calendar-months-cancel lotus-calendar-months-cancel2">
					<image @tap="setStauts('showYearFlag',false);" class="lotus-calendar-months-cancel-icon" src="data:image/png;base64,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" mode="aspectFit"></image>
				</view> -->
      </view>
      <view style="padding-top: 20rpx" class="lotus-calendar-months-box">
        <view v-for="(item, index) in yearArray" :key="index" :style="item.flag && calendarData.monthStyle" class="lotus-calendar-months-text lotus-calendar-months-text2">
          <text @tap="setAct(item, 'year')" v-text="item.y"></text>
        </view>
      </view>
    </view>
    <view v-if="calendarData.isShow" @tap="clickShowCalendar" class="lotus-calendar-mask"></view>
  </view>
</template>
<style lang="less">
@import './Winglau14-lotusCalendar.css';
</style>
<script>
export default {
  name: 'lotus-calendar',
  props: ['calendarData'],
  data() {
    return {
      isShow: true,
      weekText: ['一', '二', '三', '四', '五', '六', '日'],
      aMonth: [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31],
      showMonthFlag: false,
      fullFlag: false,
      monthArray: [
        {
          monthText: '01月',
          showCurMonth: '1',
          flag: false,
        },
        {
          monthText: '02月',
          showCurMonth: '2',
          flag: false,
        },
        {
          monthText: '03月',
          showCurMonth: '3',
          flag: false,
        },
        {
          monthText: '04月',
          showCurMonth: '4',
          flag: false,
        },
        {
          monthText: '05月',
          showCurMonth: '5',
          flag: false,
        },
        {
          monthText: '06月',
          showCurMonth: '6',
          flag: false,
        },
        {
          monthText: '07月',
          showCurMonth: '7',
          flag: false,
        },
        {
          monthText: '08月',
          showCurMonth: '8',
          flag: false,
        },
        {
          monthText: '09月',
          showCurMonth: '9',
          flag: true,
        },
        {
          monthText: '10月',
          showCurMonth: '10',
          flag: false,
        },
        {
          monthText: '11月',
          showCurMonth: '11',
          flag: false,
        },
        {
          monthText: '12月',
          showCurMonth: '12',
          flag: false,
        },
      ],
      yearArray: [],
      showYearFlag: false,
      curMonthDays: 0, //当前月天数
      preMonthDays: 0, //上一个月天数
      nextMonthDays: 0, //下一个月天数
      totalDaysArr: [], //日历总天数
      curYear: 0, //当前年份
      curMonth: 0, //当前月份
      curDate: 0, //当前日份
      showCurMonth: 0, //显示日历中月份
      choseIndex: 0,
      choseCurTime: null, //每个日期obj有day/month/year
      prevYear: '', //上一年
      time: null,
      returnType: '',
      sTime: '',
      eTime: '',
      systemMonth: new Date().getMonth(), //系统当前月份
      parentDate: '',
      actFlag: false,
      savePreMonth: 0,
      savePreYear: 0,
      startYear: 0,
      endYear: 0,
      selectTime: '',
    }
  },
  components: {},
  computed: {
    checkStatus() {
      const _this = this
      const t = _this.calendarData.isShow
      _this.fullFlag = true
      if (t && !_this.actFlag) {
        //获取父组件已选的时间值
        _this.parentDate = _this.calendarData.choseTime.split('-')
        _this.selectTime = _this.calendarData.choseTime
        _this.totalDaysArr = []
        //记录父组件的年份与月份的值，用于年月日比对上当前日期高亮
        _this.savePreMonth = _this.parentDate[1] * 1 - 1
        _this.savePreYear = _this.parentDate[0] * 1
        //显示日历组件并生成对应数据
        _this.show(_this.parentDate[0] * 1, _this.parentDate[1] * 1 - 1, _this.parentDate[2] * 1)
      }
      return t
    },
  },
  methods: {
    //点击上一月
    clickPrevMonth() {
      //console.log('prev'+this.curMonth);
      this.totalDaysArr = []
      this.actFlag = true
      //缓存当前切换月份，用于当前日期高亮判断
      const tMonth = this.curMonth
      this.savePreMonth = tMonth
      //判断点击到一月进入一年设置月份为12月
      if (this.curMonth <= 0) {
        this.curMonth = 11
        this.curYear--
      } else {
        this.curMonth--
      }
      //判断当前切换的月份是否与父组件传的月份一致，用于当前日期高亮
      const pMonth = this.calendarData.choseTime.split('-')[1] * 1 - 1
      if (this.curMonth === pMonth) {
        this.savePreMonth = pMonth
      }
      //生成日历数据
      this.createCalendarData(this.curYear, this.curMonth, this.curDate)
    },
    //点击下一月
    clickNextMonth() {
      this.totalDaysArr = []
      //缓存当前切换月份，用于当前日期高亮判断
      const tMonth = this.curMonth
      this.savePreMonth = tMonth
      this.curMonth++
      this.actFlag = true
      //超过12月进入下一年判断设置月份为1月
      if (this.curMonth >= 12) {
        this.curMonth = 0
        this.curYear++
      }
      //判断当前切换的月份是否与父组件传的月份一致，用于当前日期高亮
      const pMonth = this.calendarData.choseTime.split('-')[1] * 1 - 1
      if (this.curMonth === pMonth) {
        this.savePreMonth = pMonth
      }
      //生成日历数据
      this.createCalendarData(this.curYear, this.curMonth, this.curDate)
    },
    //点击上一年
    clickPrevYear() {
      this.prevYear = ''
      this.totalDaysArr = []
      this.actFlag = true
      //缓存当前切换年份，用于当前日期高亮判断
      const tYear = this.curYear
      this.savePreYear = tYear
      this.curYear--
      //判断当前切换的年份是否与父组件传的年份一致，用于当前日期高亮
      const pYear = this.calendarData.choseTime.split('-')[0] * 1
      if (this.curYear === pYear) {
        this.savePreYear = pYear
      }
      //生成日历数据
      this.createCalendarData(this.curYear, this.curMonth, this.curDate)
    },
    //点击下一年
    clickNextYear() {
      this.prevYear = ''
      this.totalDaysArr = []
      //缓存当前切换年份，用于当前日期高亮判断
      const tYear = this.curYear
      this.savePreYear = tYear
      this.curYear++
      this.actFlag = true
      //判断当前切换的年份是否与父组件传的年份一致，用于当前日期高亮
      const pYear = this.calendarData.choseTime.split('-')[0] * 1
      if (this.curYear === pYear) {
        this.savePreYear = pYear
      }
      //生成日历数据
      this.createCalendarData(this.curYear, this.curMonth, this.curDate)
    },
    //选中日期
    clickTargetTime(curTime) {
      let objTime = curTime
      //遍历选中时间是否与当前时间一致，一致flag = true or flag = false;
      //console.log(this.totalDaysArr);
      this.totalDaysArr.map((item) => {
        item.day === objTime.day && item.month === objTime.month ? (item.flag = true) : (item.flag = false)
      })
      //为了不影响objTime这个数据源，定义一个临时obj
      let tempObj = {}
      //处理选中的时间
      for (let i in objTime) {
        if (i === 'day') {
          tempObj.day = objTime[i]
          tempObj.day = tempObj.day < 10 ? `0${tempObj.day}` : tempObj.day
        }
        if (i === 'month') {
          //选中时间跨入上一年12月份
          tempObj.month = objTime[i]
          if (objTime[i] < 0) {
            tempObj.month = 12
          } else {
            tempObj.month = tempObj.month + 1 < 10 ? `0${tempObj.month + 1}` : tempObj.month + 1
          }
        }
        if (i === 'year') {
          tempObj.year = objTime[i]
        }
      }

      this.choseCurTime = tempObj

      //判断选择时间跨入了下一年换算当前月份
      if (this.choseCurTime.month > 12) {
        this.choseCurTime.month = this.choseCurTime.month - 12 < 10 ? `0${this.choseCurTime.month - 12}` : this.choseCurTime.month - 12
      }
      this.calendarData.isShow = false
      //用于computed计算标识不然函数会再次执行
      this.actFlag = false
      this.calendarData.choseTime = `${this.choseCurTime.year}-${this.choseCurTime.month}-${this.choseCurTime.day}`
      this.selectTime = this.calendarData.choseTime
      //传值给父组件
      // this.$emit('returnDate',{time:`${this.choseCurTime.year}-${this.choseCurTime.month}-${this.choseCurTime.day}`,timeType:this.calendarData.type,isShow:false});
    },
    //显示与隐藏日历
    clickShowCalendar() {
      this.showMonthFlag = false
      this.showYearFlag = false
      this.actFlag = false
      this.$emit('closeCalendar', {
        time: this.selectTime,
        isShow: false,
      })
    },
    //显示
    show(curYear, curMonth, curDate) {
      this.returnType = this.calendarData.type
      this.totalDaysArr = []
      const objDate = new Date()
      //当前年份
      this.curYear = curYear || objDate.getFullYear()
      //当前月份
      if (curMonth === 0 || curMonth) {
        this.curMonth = curMonth
      } else {
        this.curMonth = objDate.getMonth()
      }
      //当前几号
      this.curDate = curDate || objDate.getDate()
      //生成对应年份区间列表数据
      this.crateYearRange()
      //生成日历数据
      this.createCalendarData(this.curYear, this.curMonth, this.curDate)
    },
    //方法调用
    lotusCalendar() {
      this.show()
    },
    //是否为闰年
    isLeapYear(year) {
      return (year % 4 === 0 && year % 100 !== 0) || year % 400 === 0
    },
    //获取当前月份天数
    getCurMonthDays(year, month) {
      //console.log(year, month);
      if (this.isLeapYear(year)) {
        //判断当前月份是2月
        if (this.curMonth === 1 && this.curMonth === month) {
          return (this.aMonth[1] = 29)
        } else {
          return this.aMonth[month]
        }
      } else {
        //不是闰年重置2月天数为28
        this.aMonth[1] = 28
        return this.aMonth[month]
      }
    },
    //自动补0
    autoPatchZero(val) {
      return val < 10 ? `0${val}` : val
    },
    //生成日历数据 curYear年份 curMonth月份 curDate号数
    createCalendarData(curYear, curMonth, curDate) {
      //换算当前月份自动补全 val < 10 ? `0${val}` : val;
      this.showCurMonth = curMonth
      this.showCurMonth = this.autoPatchZero(this.showCurMonth + 1)
      let preYear = ''
      //当前月份1号星期几
      let iWeek = new Date(curYear, curMonth, 1).getDay()
      if (iWeek === 0) {
        iWeek = 7
      }
      if (curMonth === 0) {
        const t = 12
        preYear = curYear
        preYear -= 1
        this.prevYear = preYear
        //上一个月份共多少天
        this.preMonthDays = this.getCurMonthDays(preYear, t - 1)
      } else {
        //上一个月份共多少天
        this.preMonthDays = this.getCurMonthDays(curYear, curMonth - 1)
      }
      //当前月份共多少天
      this.curMonthDays = this.getCurMonthDays(curYear, curMonth)
      //下一个月份共多少天
      this.nextMonthDays = this.getCurMonthDays(curYear, curMonth + 1)
      //上一个月剩下几天
      for (let i = iWeek - 2; i >= 0; i--) {
        const obj = {
          day: this.preMonthDays - i,
          className: 'gray',
          type: 0,
          flag: false,
          month: curMonth - 1,
          year: preYear ? preYear : curYear,
        }
        this.totalDaysArr.push(obj)
      }

      //当前月份天数
      for (let i = 1; i <= this.curMonthDays; i++) {
        const obj = {
          day: i,
          type: 1,
          flag: false,
          className: '',
          month: curMonth,
          year: curYear,
        }
        if (i === curDate && curMonth === this.savePreMonth && curYear === this.savePreYear) {
          obj.flag = true
          obj.className = 'act'
          obj.type = 2
        }
        this.totalDaysArr.push(obj)
      }

      //下一个月开始的几天
      const leaveLength = 42 - this.totalDaysArr.length
      for (let i = 1; i <= leaveLength; i++) {
        const obj = {
          day: i,
          className: 'gray',
          type: 0,
          flag: false,
          month: curMonth + 1,
          year: curYear,
        }
        //点击月份进入了下一年
        if (obj.month >= 12) {
          obj.year += 1
        }
        this.totalDaysArr.push(obj)
      }
    },
    //月份/年份设置高亮
    setAct(obj, type) {
      if (type === 'month') {
        this.monthArray.map((item, index) => {
          item.flag = false
        })
        this.curMonth = obj.showCurMonth * 1 - 1
        this.setStauts('showMonthFlag', false)
      } else {
        this.yearArray.map((item, index) => {
          item.flag = false
        })
        this.setStauts('showYearFlag', false)
        this.curYear = obj.y
      }
      obj.flag = true
      this.totalDaysArr = []
      this.createCalendarData(this.curYear, this.curMonth, this.curDate)
      this.selectTime = `${this.curYear}-${this.curMonth + 1 < 10 ? '0' + (this.curMonth + 1) : this.curMonth + 1}-${this.curDate < 10 ? '0' + this.curDate : this.curDate}`
      this.actFlag = true
      this.fullFlag = true
    },
    //更改状态
    setStauts(type, obj) {
      //月份选中
      if (type === 'showMonthFlag' && obj) {
        this.monthArray.map((item, index) => {
          item.flag = false
          if (item.showCurMonth * 1 - 1 === this.curMonth) {
            item.flag = true
          }
        })
      } else {
        //年份选中
        this.yearArray.map((item, index) => {
          item.flag = false
          if (item.y === this.curYear) {
            item.flag = true
          }
        })
      }
      this[type] = obj
      this.fullFlag = false
    },
    //年份区间数据创建
    crateYearRange(year) {
      this.yearArray = []
      this.startYear = this.curYear - 15
      this.endYear = this.curYear + 5
      this.actFlag = true
      for (let i = this.startYear + 1; i < this.startYear + 15; i++) {
        let sObj = {
          y: i,
          flag: false,
        }
        //判断切换年份是否与之前年份一致&高亮
        if (i === this.savePreYear) {
          sObj.flag = true
        }
        this.yearArray.push(sObj)
      }
      for (let j = this.curYear; j <= this.curYear + 5; j++) {
        let eObj = {
          y: j,
          flag: false,
        }
        //判断切换年份是否与之前年份一致&高亮
        if (j === this.savePreYear) {
          eObj.flag = true
        }
        this.yearArray.push(eObj)
      }
    },
    //年份区间变化
    yearRangeChanage(type) {
      this.curYear += type
      this.crateYearRange()
    },
    // 点击确定按钮
    confirmFn() {
      this.showMonthFlag = false
      this.showYearFlag = false
      this.actFlag = false
      let option = {
        isShow: false,
      }
      if (this.choseCurTime) {
        option.time = `${this.choseCurTime.year}-${this.choseCurTime.month}-${this.choseCurTime.day}`
        option.timeType = this.calendarData.type ? this.calendarData.type : ''
      }
      if (this.selectTime) {
        option.time = this.selectTime
      }
      this.$emit('returnDate', {
        ...option,
      })
    },
  },
}
</script>
