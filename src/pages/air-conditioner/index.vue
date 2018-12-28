<template>
  <div @touchstart="viewTouchStart" class="air-conditioner-index w-full h-full">
    <div class="contron-head w-full pos-re">
      <div class="ruler-bg">
        <div class="air-icon-box dis-flex jc-c">
          <div class="air-temperature">
            36
          </div>
          <div class="air-temperature-icon">
            ℃
          </div>
        </div>
        <img :src="mainImg" class="ruler-bg-img" alt="">
        <div class="cilcle-icon-box">
          <!-- <div>
            <div class="cilcle-icon"></div>
          </div> -->
          <i 
            @touchmove="viewTouchMove"
            @touchstart="viewTouchStart"
            @touchend="viewTouched"
            @touchcancel="viewTouched"
            :style="{ transform: 'rotate(' + rotates + 'deg)'}">
            <!-- :style="{width: size + 'rpx', height: size + 'rpx', left: posSize + 'rpx', top: posSize + 'rpx'}" -->
            <span
              
              :style="{width: size + 'rpx', height: size + 'rpx', left: 152 + 'rpx', top: 320 + 'rpx'}"
            >
            </span>
          </i>
        </div>
        <p class="room-temperature tc f12">室温{{roomTemperature}}℃</p>
      </div>
      <div class="air-type-box">
        <div class="air-type f12 tc">
          制冷模式
        </div>
      </div>
    </div>
    <div class="pos-re">
      <div class="contron-middle bsbb dis-flex jc-bet alicen pb20">
        <div>
          <img :src="middleLeftImg" style="width: 50rpx; height: 46rpx" alt="">
        </div>
        <div class="br97 bsbb" style="width: 12rpx; height: 12rpx; border-radius: 50%;"></div>
        <div class="br97 bsbb" style="width: 20rpx; height: 20rpx; border-radius: 50%;"></div>
        <div class="bg00A4A2 bsbb" style="width: 26rpx; height: 26rpx; border-radius: 50%;"></div>
        <div>
          <img :src="middleRightImg" style="width: 52rpx; height: 56rpx" alt="">
        </div>
      </div>
      <!-- <div style="width: 160rpx; height: 160rpx; background: red; position: absolute; left: 295rpx; bottom: -1rpx"> -->
        <div class="bsbb off-btn-box dis-flex jc-c">
          <img :src="offBtn" style="width: 120rpx; height: 120rpx;" alt="">
        </div>
      <!-- </div> -->
    </div>
    <div class="contron-foter bsbb dis-flex jc-bet alicen pt20">
      <div class="mt20">
        <img :src="fllterImg1" style="width: 72rpx; height: 72rpx" alt="">
      </div>
      <div class="mt20">
        <img :src="fllterImg2" style="width: 72rpx; height: 72rpx" alt="">
      </div>
      <div class="mt20">
        <img :src="fllterImg3" style="width: 72rpx; height: 72rpx" alt="">
      </div>
      <div class="mt20">
        <img :src="fllterImg4" style="width: 72rpx; height: 72rpx" alt="">
      </div>
      <div class="mt20">
        <img :src="fllterImg5" style="width: 72rpx; height: 72rpx" alt="">
      </div>
    </div>
  </div>
</template>
<script>
import './wxss/index.wxss'
export default {
  data() {
    return {
      renterName: '',
      mainImg: '/static/img/hotBg.png',
      roomTemperature: '36',
      middleLeftImg: '/static/img/threePart.png',
      middleRightImg: '/static/img/sexPart.png',
      fllterImg1: '/static/img/autoHot.png',
      fllterImg2: '/static/img/snow.png',
      fllterImg3: '/static/img/hot.png',
      fllterImg4: '/static/img/walter.png',
      fllterImg5: '/static/img/wind.png',
      offBtn: '/static/img/off.png',
      rotateStart: 45,
      rotates: 45,
      startX: 0,
      startY: 0,
      size: 20,
      posSize: 40,
      coordinateX: 187, // 191
      coordinateY: 107, // 104
      zeroX: 0,
      zeroY: 0,
      startR: false,
      endR: false
    }
  },
  onLoad() {
    wx.getSystemInfo({
      success: (res) => {
        console.log(res, 'res')
        const { screenWidth } = res;
        this.coordinateY = parseInt(screenWidth / 750 * 206)
        // this.coordinateX = parseInt(screenWidth / 750 * 208)
        this.coordinateX = parseInt(screenWidth / 750 * 370)
        this.zeroX = parseInt(screenWidth / 750 * 370)
        this.zeroY = parseInt(screenWidth / 750 * 372)
      }
    })
  },
  methods: {
    viewTouchMove(even){
      
      //  开始 
      const { startX, startY, rotates, rotateStart, coordinateX, coordinateY, zeroX, zeroY } = this.$data;
      const { pageX, pageY, clientX, clientY } = even;
      const startQur = startX - zeroX;
      const startYQur = startY - zeroY;
      const pageQur = pageX - zeroX;
      const pageYQur = pageY - zeroY;
      let reg = 0, reg1 = 0, reg2 = 0;
      
      if (startYQur > 0) {
        reg1 = Math.atan2((zeroY - startY), (startX - zeroX)) * 180 / (Math.PI) + 360; //
      } else {
        reg1 = Math.atan2((zeroY - startY), (startX - zeroX)) * 180 / (Math.PI); //
      }
      if (pageYQur > 0) {
        reg2 = Math.atan2((zeroY - pageY), (pageX - zeroX)) * 180 / (Math.PI) + 360; // 
      } else {
        reg2 = Math.atan2((zeroY - pageY), (pageX - zeroX)) * 180 / (Math.PI)
      }

      reg = reg2 - reg1;
      this.rotates = (rotateStart - (reg % 360) + 360) % 360;
      if (this.rotates > 315) this.rotates = 315
      if (this.rotates < 45) this.rotates = 45
    },
    viewTouchStart(even) {
      const { coordinateX, coordinateY } = this.$data;
      // console.log(even);
      const { pageX, pageY, clientX, clientY } = even;
       console.log(Math.atan2(pageY, pageX), '开始')
      // if (pageX > coordinateX && pageY > coordinateY)
      this.startX = pageX;
      this.startY = pageY;
      this.startR = pageX >= coordinateX;
      console.log( '开始触摸', pageX, pageY, clientX, clientY )
      // console.log('开始触摸',  even.mp.changedTouches[0], coordinateX, coordinateY)
      // this.size = 40,
      // this.posSize = 40
    },
    viewTouched(even) {
      // console.log(even, '=====================')
      const { coordinateX, coordinateY, rotates } = this.$data;
      let newRotates = rotates;
      // if (newRotates > 175 ) newRotates = 175;
      // if (newRotates < )
      const { pageX, pageY, clientX, clientY } = even.mp.changedTouches[0];
      // console.log(Math.atan2(pageY, pageX), '结束 ')
      this.startX = pageX;
      this.startY = pageY;
      this.rotates = rotates;
      this.rotateStart = rotates;
      this.endR = pageX >= coordinateX;
      console.log('结束触摸',  this.endR)
      this.size = 20,
      this.posSize = 40;
      console.log(this.rotates, '======================')
    }
  }

}
</script>
