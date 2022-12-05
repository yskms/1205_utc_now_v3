<script>
// import HelloWorld from './components/HelloWorld.vue'
import Datepicker from 'vuejs-datepicker';

export default {
  name: 'App',
  components: {
    Datepicker
  },
  data(){
    return{
      localDate:null,
      localHour:null,
      localMin:null,
      utcDate:null,
      utcHour:null,
      utcMin:null,
      hourArr:[0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,],
      minArr:[0,15,30,45,],
      isHour:false,
      isMin:false,

      offset:null,
      isLocal:true,

      // latitude: 0,
      // longitude: 0,
      // timestamp:null,
    }
  },
  mounted(){
    this.localDate = new Date()
    this.localHour = this.localDate.getHours()
    this.localMin = this.localDate.getMinutes()
    this.offset = this.localDate.getTimezoneOffset()
    console.log(this.offset/60)

    const localDateCopy = new Date()
    this.utcDate = new Date(localDateCopy.setHours(localDateCopy.getHours() + this.offsetHour))
    this.utcHour = this.utcDate.getHours()
    this.utcMin = this.utcDate.getMinutes()
  },
  computed:{
    offsetHour(){
      return this.offset /60
    }
  },
  watch:{
    localDate(newVal, oldVal){
      if(this.isLocal){
      console.log(newVal, oldVal)
      this.changeLocalDate(newVal)
      }
    },
    localHour(newVal, oldVal){
      if(this.isLocal){
      console.log(newVal, oldVal)
      this.changeLocalHour(newVal)
      }
    },
    localMin(newVal, oldVal){
      if(this.isLocal){
      console.log(newVal, oldVal)
      this.changeLocalMin(newVal)
      }
    },
    // 両方watchしたらループしてまうw
    utcDate(newVal, oldVal){
      if(!this.isLocal){
        console.log(newVal, oldVal)
        this.changeUtcDate(newVal)
      }
    },
    utcHour(newVal, oldVal){
      if(!this.isLocal){
        console.log(newVal, oldVal)
        this.changeUtcHour(newVal)
      }
    },
    utcMin(newVal, oldVal){
      if(!this.isLocal){
        console.log(newVal, oldVal)
        this.changeUtcMin(newVal)
      }
    },
  },
  methods:{
    changeLocalDate(newVal){
      const local = new Date(newVal)//そのままnewVal使うとリンクしてthis.localDateまで変わってしまうのでnewする
      const utc = local.setHours(local.getHours() + this.offsetHour)
      this.utcDate = new Date(utc)
      console.log(this.utcDate)
    },
    changeLocalHour(newVal){
      this.localDate.setHours(newVal)
      const localDateCopy = new Date(this.localDate)
      this.utcDate = new Date(localDateCopy.setHours(localDateCopy.getHours() + this.offsetHour))
      this.utcHour = this.utcDate.getHours()
      this.utcMin = this.utcDate.getMinutes()
    },
    changeLocalMin(newVal){
      this.localDate.setMinutes(newVal)
      const localDateCopy = new Date(this.localDate)
      this.utcDate = new Date(localDateCopy.setHours(localDateCopy.getHours() + this.offsetHour))
      this.utcHour = this.utcDate.getHours()
      this.utcMin = this.utcDate.getMinutes()
    },
    // コピペしただけやからconse localとconst utcは逆ですよ！
    changeUtcDate(newVal){
      const local = new Date(newVal)//そのままnewVal使うとリンクしてthis.localDateまで変わってしまうのでnewする
      const utc = local.setHours(local.getHours() - this.offsetHour)
      this.localDate = new Date(utc)
      console.log(this.localDate)
    },
    changeUtcHour(newVal){
      this.utcDate.setHours(newVal)
      const localDateCopy = new Date(this.utcDate)
      this.localDate = new Date(localDateCopy.setHours(localDateCopy.getHours() - this.offsetHour))
      this.localHour = this.localDate.getHours()
      this.localMin = this.localDate.getMinutes()
    },
    changeUtcMin(newVal){
      this.utcDate.setMinutes(newVal)
      const localDateCopy = new Date(this.utcDate)
      this.localDate = new Date(localDateCopy.setHours(localDateCopy.getHours() - this.offsetHour))
      this.localHour = this.localDate.getHours()
      this.localMin = this.localDate.getMinutes()
    },
    selectHour(n,tf){
      if(tf){  //isLocalがtrueなら
        this.localHour = n
        this.isHour = false
      }else{
        this.utcHour = n
        this.isHour = false
      }
    },
    selectMin(n,tf){
      if(tf){
        this.localMin = n
        this.isMin = false
      }else{
        this.utcMin = n
        this.isMin = false
      }
    },
    clickHour(){
      this.isHour = !this.isHour
      this.isMin = false
    },
    clickMin(){
      this.isMin = !this.isMin
      this.isHour = false
    },

    // getLocation () {
    //     if (!navigator.geolocation) {
    //       alert('現在地情報を取得できませんでした。お使いのブラウザでは現在地情報を利用できない可能性があります。エリアを入力してください。')
    //       return
    //     }
    //     const options = {
    //       enableHighAccuracy: false,
    //       timeout: 5000,
    //       maximumAge: 0
    //     }
    //     navigator.geolocation.getCurrentPosition(this.success, this.error, options)
    // },
    // success (position) {
    //   this.latitude = position.coords.latitude
    //   this.longitude = position.coords.longitude
    //   this.timestamp = position.timestamp
    // },
    // error (error) {
    //   switch (error.code) {
    //     case 1: //PERMISSION_DENIED
    //       alert('位置情報の利用が許可されていません')
    //       break
    //     case 2: //POSITION_UNAVAILABLE
    //       alert('現在位置が取得できませんでした')
    //       break
    //     case 3: //TIMEOUT
    //       alert('タイムアウトになりました')
    //       break
    //     default:
    //       alert('現在位置が取得できませんでした')
    //       break
    //   }
    // },
}}
</script>

<template>
  <div class="cont">
    <div class="main" v-show="isLocal">
      <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
      <div class="local_main">
        <p>local time</p>
        <div class="local_wrap">
            <div class="select_hour">
              <p @click="selectHour(h,true)" v-for="h in hourArr" :key="h" v-show="isHour">{{h}}</p>
            </div>
            <div class="select_min">
              <p @click="selectMin(m,true)" v-for="m in minArr" :key="m" v-show="isMin">{{m}}</p>
            </div>
          <Datepicker v-model="localDate"
            name="local"/>
          <div @click="clickHour">{{localHour}}</div>
          <div>:</div>
          <div @click="clickMin">{{localMin}}</div>
        </div>
      </div>
<!-- ------------------------------ -->
      <div class="change" @click="isLocal=!isLocal">
        Reverse icon
      </div>
<!-- ------------------------------ -->
      <div class="utc_main">
        <p>utc time</p>
        <div class="utc_wrap">
            <Datepicker v-model="utcDate"
              name="utc"
              disabled />
            <div>{{utcHour}}</div>
            <div>:</div>
            <div>{{utcMin}}</div>
        </div>
      </div>
    </div>

<!-- ------------------------------------------------------------------------- -->
    <div class="main" v-show="!isLocal">
      <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
      <div class="local_main">
        <p>utc time</p>
        <div class="local_wrap">
            <div class="select_hour">
              <p @click="selectHour(h,false)" v-for="h in hourArr" :key="h" v-show="isHour">{{h}}</p>
            </div>
            <div class="select_min">
              <p @click="selectMin(m,false)" v-for="m in minArr" :key="m" v-show="isMin">{{m}}</p>
            </div>
          <Datepicker v-model="utcDate"
            name="local"/>
          <div @click="clickHour">{{utcHour}}</div>
          <div>:</div>
          <div @click="clickMin">{{utcMin}}</div>
        </div>
      </div>
<!-- ------------------------------ -->
      <div class="change" @click="isLocal=!isLocal">
        Reverse icon
      </div>
<!-- ------------------------------ -->
      <div class="utc_main">
        <p>local time</p>
        <div class="utc_wrap">
            <Datepicker v-model="localDate"
              name="utc"
              disabled />
            <div>{{localHour}}</div>
            <div>:</div>
            <div>{{localMin}}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.local_main{
  background-color: blanchedalmond;
}
.utc_main{
  background-color: blanchedalmond;
}
.local_wrap{
  display: flex;
  position: relative;
}
.utc_wrap{
  display: flex;
}
.select_hour{
  position: absolute;
  top: 1.5em;
  left: 0;
  background-color: beige;
  width: 300px;
  /* border: 1px solid #ccc; */
}
.select_min{
  position: absolute;
  top: 1.5em;
  left: 0;
  background-color: beige;
  width: 300px;
  /* border: 1px solid #ccc; */
}
</style>
