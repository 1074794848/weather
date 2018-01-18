<template>
  <div class="weather">
    <div class="content_top">
      <h1>{{weatherData.weather}}</h1>
      <h2>{{cityData.province}} {{cityData.city}}</h2>
      <p>{{weatherData.data | intercept}}</p>
      <img src="../../static/img/logo_e.png" width="100%" alt="">
    </div>
    <div class="content_bottom">
      <ul>
        <li v-for="(complets,index) in completData">
          <a @click="GoToInfo(index)" class="Onetime">
            <span>{{complets.date | interceptDate}}</span>
            <span>{{complets.temperature}}</span>
          </a>
        </li>
        <!--<li>-->
          <!--<a href="" class="Towtime">-->
            <!--<span>周二</span>-->
            <!--<span>15&deg-20&deg</span>-->
          <!--</a>-->
        <!--</li>-->
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  mounted(){
    this.GetUserIpAddress()
  },
  name: 'HelloWorld',
  data () {
    return {
      cityData:{},
      completData:[],
      weatherData:{
        weather:"",
        date:""
      }

    }
  },
  filters: {
    // 过滤截取实时天气数据
    intercept(val){
      return val.slice(-3,-1)
    },
    interceptDate(val){
      return val.slice(0,2)
    }
  },

  methods: {
    GetUserIpAddress(){
      this.$http.get(this.$config.GetUserIp)
        .then(success=>{
          this.GetUserAddress(success.data.ip)
        })
        .catch(error=>{
          $.toast("请检查你的网络")
        })
    },
    GetUserAddress(ip){
      this.$http.jsonp(this.$config.GetUserAddress,{
        params:{
          ak:this.$config.ipConf.ak,
          ip:ip
        }
      })
        .then(success=>{
          this.cityData=success.data.content.address_detail
          this.GetWeatherData()
        })
        .catch(error=>{
          $.toast("请检查网络")
        })
    },
    GetWeatherData(){
      this.$http.jsonp(this.$config.GetWeather,{
        params:{
          location:this.cityData.city,
          output:this.$config.ipConf.output,
          ak:this.$config.ipConf.ak
        }
      })
        .then(success=>{
          let wea=success.data.results[0].weather_data[0]
          this.weatherData.weather=wea.weather
          this.weatherData.data=wea.date
          this.completData=success.data.results[0].weather_data

        })
        .catch(error=>{
          $.toast("请检查你的网络")
        })

    },
    GoToInfo(id){
      localStorage.setItem("weatherdata",JSON.stringify(this.weatherData.completData))
      this.$router.push({name:"Info",params:{id:id}})
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .weather{
    text-align: center;
    height: 100%;
    overflow:overlay;
  }

  .content_top{
    padding-top: 80px;
    color: #fff;
    background: -webkit-linear-gradient(#dd4879 , #fdd73f);
    background: -o-linear-gradient(#dd4879 , #fdd73f);
    background: -moz-linear-gradient(#dd4879 , #fdd73f);
    background: linear-gradient(#dd4879 , #fdd73f);
    height: 80%;
    position: relative;
  }
  .content_top h1{
    font-weight: 500;
    margin:5px 0px;
  }
  .content_top h2{
    font-size: 20px;
    font-weight: 400;
  }
  .content_top p{
    font-size: 96px;
    margin-top: 10px;
  }
  .content_top img{
    margin-top: 98px;
    position: absolute;
    left: 0;
    bottom: 0;
  }
  .content_bottom ul li{
    border-bottom: 1px solid #ccc;
  }
  .content_bottom ul li a{
    overflow:hidden ;
    display: block;
    margin: 23px 30px;
    text-align: center;
  }
  .content_bottom ul li:first-child a{
    color: #ff6666;
  }
  .Onetime span:first-child{
    float: left;
  }
  .Onetime span:last-child{
    float: right;
  }
  .Towtime span:first-child{
    float: left;
  }
  .Towtime span:last-child{
    float: right;
  }
</style>
