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
      hourArr:[0,1,2,3,4,5,6,],
      minArr:[15,30,45,],
      isHour:false,
      isMin:false,

      latitude: 0,
      longitude: 0,
      timestamp:null,
    }
  },
  mounted(){
    this.localDate = new Date()
    this.localHour = this.localDate.getHours()
    this.localMin = this.localDate.getMinutes()

    const localDateCopy = new Date()
    this.utcDate = new Date(localDateCopy.setHours(localDateCopy.getHours() - 14))
    this.utcHour = this.utcDate.getHours()
    this.utcMin = this.utcDate.getMinutes()
  },
  watch:{
    localDate(newVal, oldVal){
      console.log(newVal, oldVal)
      this.changeLocalDate(newVal)
    },
    localHour(newVal, oldVal){
      console.log(newVal, oldVal)
      this.changeLocalHour(newVal)
    },
    localMin(newVal, oldVal){
      console.log(newVal, oldVal)
      this.changeLocalMin(newVal)
    },
    // 両方watchしたらループしてまうw
    // utcDate(newVal, oldVal){
    //   console.log(newVal, oldVal)
    // this.changeUtcDate(newVal)
    // },
  },
  methods:{
    changeLocalDate(newVal){
      const local = new Date(newVal)//そのままnewVal使うとリンクしてthis.localDateまで変わってしまうのでnewする
      const utc = local.setHours(local.getHours() - 14)
      this.utcDate = new Date(utc)
      console.log(this.utcDate)
    },
    changeLocalHour(newVal){
      this.localDate.setHours(newVal)
      const localDateCopy = new Date(this.localDate)
      this.utcDate = new Date(localDateCopy.setHours(localDateCopy.getHours() - 14))
      this.utcHour = this.utcDate.getHours()
      this.utcMin = this.utcDate.getMinutes()
    },
    changeLocalMin(newVal){
      this.localDate.setMinutes(newVal)
      const localDateCopy = new Date(this.localDate)
      this.utcDate = new Date(localDateCopy.setHours(localDateCopy.getHours() - 14))
      this.utcHour = this.utcDate.getHours()
      this.utcMin = this.utcDate.getMinutes()
    },
    selectHour(n){
      this.localHour = n
      this.isHour = false
    },
    selectMin(n){
      this.localMin = n
      this.isMin = false
    },

    getLocation () {
        if (!navigator.geolocation) {
          alert('現在地情報を取得できませんでした。お使いのブラウザでは現在地情報を利用できない可能性があります。エリアを入力してください。')
          return
        }
        const options = {
          enableHighAccuracy: false,
          timeout: 5000,
          maximumAge: 0
        }
        navigator.geolocation.getCurrentPosition(this.success, this.error, options)
    },
    success (position) {
      this.latitude = position.coords.latitude
      this.longitude = position.coords.longitude
      this.timestamp = position.timestamp
    },
    error (error) {
      switch (error.code) {
        case 1: //PERMISSION_DENIED
          alert('位置情報の利用が許可されていません')
          break
        case 2: //POSITION_UNAVAILABLE
          alert('現在位置が取得できませんでした')
          break
        case 3: //TIMEOUT
          alert('タイムアウトになりました')
          break
        default:
          alert('現在位置が取得できませんでした')
          break
      }
    },
}}
</script>

<template>
  <div class="cont">
    <div class="main">
      <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
      <div class="local_wrap">
          <div class="select_hour">
            <p @click="selectHour(h)" v-for="h in hourArr" :key="h" v-show="isHour">{{h}}</p>
          </div>
          <div class="select_min">
            <p @click="selectMin(m)" v-for="m in minArr" :key="m" v-show="isMin">{{m}}</p>
          </div>
        <Datepicker v-model="localDate"
          name="local"/>
        <div @click="isHour=!isHour">{{localHour}}</div>
        <div>:</div>
        <div @click="isMin=!isMin">{{localMin}}</div>
        <button @click="getLocation">aaa</button>
      </div>
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
</template>

<style scoped>
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
