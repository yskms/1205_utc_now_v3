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

    }
  },
  mounted(){
    this.localDate = new Date() //localDateにDateオブジェクトを作る。たくさんのDateなデータが入っている。
    this.localHour = this.localDate.getHours()  //なので、そこからget〜で色々取り出します
    this.localMin = this.localDate.getMinutes()
    this.offset = this.localDate.getTimezoneOffset()
    console.log(this.offset/60)

    const localDateCopy = new Date()  //この下のset〜を使うとデータが更新されてしまうので、コピーを作って対応するパターンです
    this.utcDate = new Date(localDateCopy.setHours(localDateCopy.getHours() + this.offsetHour))
    //ミリ秒？とかの数字でデータを返された時は、上記のようにnew Date()にぶち込めば変換できます
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
      console.log(this.localDate)
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
    // コピペしただけやからconse localとconst utcは逆ですよ！-------------------
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
    // ----------------------------------------------------------------------
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
}}
</script>

<template>
  <div class="cont">
    <div class="title">
      <svg xmlns="http://www.w3.org/2000/svg" width="16" height="20" fill="currentColor" class="bi bi-watch" viewBox="0 0 16 16">
        <path d="M8.5 5a.5.5 0 0 0-1 0v2.5H6a.5.5 0 0 0 0 1h2a.5.5 0 0 0 .5-.5V5z"/>
        <path d="M5.667 16C4.747 16 4 15.254 4 14.333v-1.86A5.985 5.985 0 0 1 2 8c0-1.777.772-3.374 2-4.472V1.667C4 .747 4.746 0 5.667 0h4.666C11.253 0 12 .746 12 1.667v1.86a5.99 5.99 0 0 1 1.918 3.48.502.502 0 0 1 .582.493v1a.5.5 0 0 1-.582.493A5.99 5.99 0 0 1 12 12.473v1.86c0 .92-.746 1.667-1.667 1.667H5.667zM13 8A5 5 0 1 0 3 8a5 5 0 0 0 10 0z"/>
      </svg>
      UTC NOW
    </div>
    <div class="footer">
      {{localDate}}
    </div>
    <div class="main" v-show="isLocal">
      <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
      <div class="local_main">
        <p><span>Local Time</span></p>
        <div class="local_wrap">
            <div class="select_min" v-show="isMin">
              <p @click="selectMin(m,true)" v-for="m in minArr" :key="m">{{m}}</p>
            </div>

          <Datepicker v-model="localDate" class="local_date"
            name="local"
            />
          <div class="local_hour" @click="clickHour">{{localHour}}</div>
          <!-- <div class="between"></div> -->
            <div class="select_hour" v-show="isHour">
              <p @click="selectHour(h,true)" v-for="h in hourArr" :key="h">{{h}}</p>
            </div>
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
            <!-- <div class="between">:</div> -->
            <div class="local_min">{{utcMin}}</div>
        </div>
      </div>
    </div>

<!-- ------------------------------------------------------------------------- -->
    <div class="main" v-show="!isLocal">
      <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
      <div class="local_main">
        <p><span>UTC Time</span></p>
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
          <!-- <div class="between">:</div> -->
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
            <!-- <div class="between">:</div> -->
            <div class="local_min">{{localMin}}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* body効いてない */
/* body{
  background-color: #B8BDB5;
  margin: 0 !important; 
} */
.cont{
  height: 100vh;
  /* width: 100%; */
  background-color: #fcfcfc;
  /* padding: 0 3em; */
  /* display: flex; */
  justify-content: center;
  align-items: center;
  /* background-image: url(assets/11.jpg); */
  background-size: cover;
  /* background-blend-mode: color-burn; */
  background-blend-mode: multiply;
}
.title{
  position: fixed;
  /* top: 10%; */
  font-size: 1em;
  line-height: 3rem;
  font-weight: bold;
  background-color: black;
  color: white;
  width: 100%;
  /* text-align: center; */
  padding-left: 0.5em;
}
.title svg{
  vertical-align: sub;
}
.footer{
  position: fixed;
  bottom: 0;
  width: 100%;
  font-size: 0.8em;
  background-color: black;
  color: white;
  width: 100%;
  text-align: center;
}
.main{
  /* width: 100%; */
  /* margin: 0 auto; */
  /* color: white; */
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100%;
  text-align: center;
}
/* .utc_main{ */
  /* background-color: #E0E2DB; */
  /* padding: 1em 0.5em; */
  /* width: 100%; */
  /* border-radius: 20px; */
/* } */
/* .utc_main p{
  font-size: 2em;
  font-weight: bold;
  text-align: center;
} */
.local_main,
.utc_main{
  /* background-color: #D2D4C8; */
  /* background-color: #E0E2DB; */
  /* padding: 1em 0.5em; */
  width: 100%;
  border-radius: 20px;
  padding-bottom: 2em;
}
.local_main p,
.utc_main p{
  font-size: 2em;
  font-weight: bold;
  text-align: center;
}
/* .local_main span{
  background: linear-gradient(transparent 40%, #4bd 100%);
  display: inline-block;
  padding: 0 10px 0 0;
} */
.local_wrap,
.utc_wrap{
  display: flex;
  width: 80%;
  margin: 0 auto;
  /* position: relative; */
}
.local_date,
.utc_date{
  /* background-color: #E0E2DB; */
  /* padding: 1em; */
    line-height: 3rem;
  height: 3rem;
  border: 1px solid #ccc;
  border-radius: 5px;
  padding-left: .5rem;
  width: 60%;
  margin-right: .2rem;
}
/* .local_hour{ */
  /* background-color: #E0E2DB; */
  /* padding: 1em; */
  /* margin-left: 1em;
  width: 2em;
  text-align: right; */
/* } */
.local_hour,
.local_min,
.utc_hour,
.utc_min{
  /* background-color: #E0E2DB; */
  /* padding: 1em; */
  /* width: 2em;
  text-align: right; */
  font-size: 1.2rem;
  border: 1px solid #ccc;
  border-radius: 5px;
  line-height: 3rem;
  height: 3rem;
  /* padding-left: .5rem; */
  width: 20%;
  margin-right: .2rem;
  position: relative;
}
.between{
  width: 1em;
  text-align: right;
}
/* .utc_wrap{
  display: flex;
} */
.select_hour{
  position: absolute;
  top: 0;
  right: 0;
  background-color: rgba(252, 252, 252, 0.7);
  width: 100%;
  /* border: 1px solid #ccc; */
  z-index: 101;
  overflow: auto;
  height: 100vh;
  text-align: center;
  font-weight: normal;
}
.select_min{
  position: absolute;
  top: 0;
  right: 0;
  background-color: rgba(252, 252, 252, 0.7);
  width: 100%;
  /* border: 1px solid #ccc; */
  z-index: 101;
  overflow: auto;
  height: 100vh;
  text-align: center;
  font-weight: normal;
      display: flex;
    flex-direction: column;
    justify-content: center;
}
.select_hour p{
  font-size: 1em;
  font-weight: normal;
}
.select_min p{
  font-size: 1em;
  font-weight: normal;
}
.change{
  padding: 0.5em;
  text-align: center;
}

</style>

<style>
.local_date div > input {
  border: none;
  border-radius: 5px;
  width: 100%;
  font-size: 1.2rem;
  background-color: #fcfcfc;
}
</style>
