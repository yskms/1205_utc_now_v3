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
        <p>Local Time</p>
        <div class="local_wrap">
            <div class="select_hour" v-show="isHour">
              <p @click="selectHour(h,true)" v-for="h in hourArr" :key="h">{{h}}</p>
            </div>
            <div class="select_min" v-show="isMin">
              <p @click="selectMin(m,true)" v-for="m in minArr" :key="m">{{m}}</p>
            </div>

          <Datepicker v-model="localDate" class="local_date"
            name="local"
            />
          <div class="local_hour" @click="clickHour">{{localHour}}</div>
          <div class="between">:</div>
          <div class="local_min" @click="clickMin">{{localMin}}</div>
        </div>
      </div>
<!-- ------------------------------ -->
      <div class="change" @click="isLocal=!isLocal">
        <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" fill="currentColor" class="bi bi-arrow-down-up" viewBox="0 0 16 16">
          <path fill-rule="evenodd" d="M11.5 15a.5.5 0 0 0 .5-.5V2.707l3.146 3.147a.5.5 0 0 0 .708-.708l-4-4a.5.5 0 0 0-.708 0l-4 4a.5.5 0 1 0 .708.708L11 2.707V14.5a.5.5 0 0 0 .5.5zm-7-14a.5.5 0 0 1 .5.5v11.793l3.146-3.147a.5.5 0 0 1 .708.708l-4 4a.5.5 0 0 1-.708 0l-4-4a.5.5 0 0 1 .708-.708L4 13.293V1.5a.5.5 0 0 1 .5-.5z"/>
        </svg>
      </div>
<!-- ------------------------------ -->
      <div class="utc_main">
        <p>UTC Time</p>
        <div class="utc_wrap">
            <Datepicker v-model="utcDate" class="local_date"
              name="utc"
              disabled />
            <div class="local_hour">{{utcHour}}</div>
            <div class="between">:</div>
            <div class="local_min">{{utcMin}}</div>
        </div>
      </div>
    </div>

<!-- ------------------------------------------------------------------------- -->
    <div class="main" v-show="!isLocal">
      <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
      <div class="local_main">
        <p>UTC Time</p>
        <div class="local_wrap">
            <div class="select_hour" v-if="isHour">
              <p @click="selectHour(h,false)" v-for="h in hourArr" :key="h">{{h}}</p>
            </div>
            <div class="select_min" v-show="isMin">
              <p @click="selectMin(m,false)" v-for="m in minArr" :key="m">{{m}}</p>
            </div>
          <Datepicker v-model="utcDate" class="local_date"
            name="local"/>
          <div class="local_hour" @click="clickHour">{{utcHour}}</div>
          <div class="between">:</div>
          <div class="local_min" @click="clickMin">{{utcMin}}</div>
        </div>
      </div>
<!-- ------------------------------ -->
      <div class="change" @click="isLocal=!isLocal">
        <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" fill="currentColor" class="bi bi-arrow-down-up" viewBox="0 0 16 16">
          <path fill-rule="evenodd" d="M11.5 15a.5.5 0 0 0 .5-.5V2.707l3.146 3.147a.5.5 0 0 0 .708-.708l-4-4a.5.5 0 0 0-.708 0l-4 4a.5.5 0 1 0 .708.708L11 2.707V14.5a.5.5 0 0 0 .5.5zm-7-14a.5.5 0 0 1 .5.5v11.793l3.146-3.147a.5.5 0 0 1 .708.708l-4 4a.5.5 0 0 1-.708 0l-4-4a.5.5 0 0 1 .708-.708L4 13.293V1.5a.5.5 0 0 1 .5-.5z"/>
        </svg>
      </div>
<!-- ------------------------------ -->
      <div class="utc_main">
        <p>Local Time</p>
        <div class="utc_wrap">
            <Datepicker v-model="localDate" class="local_date"
              name="utc"
              disabled />
            <div class="local_hour">{{localHour}}</div>
            <div class="between">:</div>
            <div class="local_min">{{localMin}}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.cont{
  height: 100vh;
  /* width: 100%; */
  background-color: #B8BDB5;
  padding: 3em;
}
.main{
  width: 100%;
  margin: 0 auto;
  /* color: white; */
}
.utc_main{
  background-color: #E0E2DB;
  padding: 0.5em;
  width: 100%;
}
.utc_main p{
  font-size: 1.5em;
  font-weight: bold;
}
.local_main{
  /* background-color: #D2D4C8; */
  background-color: #E0E2DB;
  padding: 0.5em;
  width: 100%;
}
.local_main p{
  font-size: 1.5em;
  font-weight: bold;
}
.local_wrap{
  display: flex;
  position: relative;
}
.local_date{
  background-color: #E0E2DB;
  /* padding: 1em; */
}
.local_hour{
  background-color: #E0E2DB;
  /* padding: 1em; */
  margin-left: 1em;
  width: 2em;
  text-align: right;
}
.local_min{
  background-color: #E0E2DB;
  /* padding: 1em; */
  width: 2em;
  text-align: right;
}
.between{
  width: 1em;
  text-align: right;
}
.utc_wrap{
  display: flex;
}
.select_hour{
  position: absolute;
  top: 1.5em;
  right: 0;
  background-color: white;
  width: 60%;
  /* border: 1px solid #ccc; */
  z-index: 101;
  overflow: auto;
  height: 60vh;
  text-align: center;
}
.select_hour p{
  font-size: 1em;
}
.select_min{
  position: absolute;
  top: 1.5em;
  right: 0;
  background-color: white;
  width: 60%;
  /* border: 1px solid #ccc; */
  z-index: 101;
  text-align: center;
}
.select_min p{
  font-size: 1em;
}
.change{
  padding: 0.5em;
  text-align: center;
}
</style>
