<template>
  <div class="reserve-list-index w-full h-full dis-flex jc-c">
    <div class="reserve-list-search mt30 plr20 ptb20">
      <p class="c3b f16 ptb20">预定选择</p>
      <div class="f14 c-app border-bottom-have">
        <input type="text" v-model="searchAddress" class="ptb14">
      </div>
      <div class="flex-row jc-bet border-bottom-have ptb14">
        <div class="flex-col">
          <p class="f12">入住</p>
          <picker mode="date"
            :value="startDate"
            :start="pickerStart" end="2217-09-01" @change="bindDateChangeStart($event)">
              <div class="index_picker">
                <div class="f14 c-app">{{ startDate }}</div>
              </div>
          </picker>
          <!-- <p class="f14 c-app">8月7日（周二）</p> -->
        </div>
        <div class="flex-col">
          <p class="f12">退房</p>
          <!-- <p class="f14 c-app">8月7日（周二）</p> -->
          <picker mode="date"
            :value="endDate"
            :start="pickerEnd" end="2217-09-01" @change="bindDateChangeEnd($event)">
              <div class="index_picker">
                <div class="f14 c-app">{{ endDate }}</div>
              </div>
          </picker>
        </div>
      </div>
      <div class="flex-row jc-bet border-bottom-have ptb14">
        <div class="flex-col" style="width: 200rpx">
          <p class="f12">房间数</p>
          <p class="f14 c-app dis-flex jc-str">
            <input type="number" v-model="houseNums" placeholder="请选择房间数" style="width: 60rpx">
            <span>间</span>
          </p>
        </div>
        <div class="flex-col" style="width: 200rpx">
          <p class="f12">人数</p>
          <p class="f14 c-app dis-flex jc-str">
            <input type="number" v-model="personNums" placeholder="请选择人数" style="width: 60rpx">
            <span>人</span>
          </p>
        </div>
      </div>
      <div class="dis-flex jc-c mt50">
        <div class="search-btn tc cfff f16 app-btn br10">搜索</div>
      </div>
    </div>
  </div>
</template>
<script>
import './wxss/index.wxss'
export default {
  data() {
    return {
      searchAddress: '忆泊城市艺术酒店（古墩路店）',
      startDate:'2018-05-9',
      pickerStart:'2018-05-6',
      endDate:'2018-05-10',
      pickerEnd:'2018-05-7',
      dateStart: '',
      dateEnd: '',
      houseNums: 1,
      personNums: 1,
    }
  },
  mounted(){
  // 设置时间
    this.getToday()
  },
  methods: {
    getToday(val, bol){
      let myDate;
      if (val) {
        if (!bol) {
          myDate = new Date(val);
        } else {
          console.log(this.dateStart)
          myDate = new Date(this.dateStart);
        }
      } else {
        myDate = new Date();
        this.pickerStart = myDate;
        this.pickerEnd = myDate;
      }
      let nextDate = myDate;
      if (!bol) {
        nextDate = +myDate + 1000*60*60*24;
        nextDate = new Date(nextDate)
      } else {
        nextDate = new Date(val)
      }
      console.log(myDate, nextDate)
      let myMonth = myDate.getMonth() + 1;
      let nextMonth = nextDate.getMonth() + 1;
      const weekArr = new Array("日", "一", "二", "三", "四", "五", "六");  
      if (myMonth < 10) {
        myMonth = '0'+ myMonth;  //补齐
      }
      if (nextMonth < 10) {
        nextMonth = '0'+ nextMonth;  //补齐
      }
      let mydate = myDate.getDate();
      let nextdate = nextDate.getDate();
      if (myDate.getDate()<10) {
          mydate = '0'+ myDate.getDate();  //补齐
      }
      if (nextDate.getDate()<10) {
          nextdate = '0'+ nextdate.getDate();  //补齐
      }
      const myWeek = myDate.getDay()
      const nextWeek = nextDate.getDay()
      const today = `${myMonth}月${ mydate }日 (周${weekArr[myWeek]})`;
      const nextday = `${nextMonth}月${ nextdate }日 (周${weekArr[nextWeek]})`;
      this.startDate = today
      this.endDate = nextday
    },
    bindDateChangeStart(even) {
      const { value } = even.target;
      this.dateStart = value;
      this.pickerStart = value;
      this.pickerEnd = value;
      this.getToday(value)
    },
    bindDateChangeEnd(even) {
      const { value } = even.target;
      this.dateEnd = value;
      this.getToday(value, true)
    }
  }
}
</script>
