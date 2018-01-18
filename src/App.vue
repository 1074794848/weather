<template>
  <div id="app">
    <div class="page-group">
      <div class="page">
        <!-- 网站的公共头部 -->
        <v-hedaer></v-hedaer>

        <!-- 这里是页面内容区 -->
        <router-view/>

      </div>
    </div>


    <!-- 网站的中间内容部分 -->


  </div>
</template>

<script>
  import  Headers from  "./components/header.vue";
  export default {
    name:"app",
    components:{
      "v-hedaer":Headers
    }
  }
</script>

<style>
  *{
    margin: 0;
    padding: 0;
    list-style: none;
  }
  h1, h2 {
    font-size: 30px;
    font-weight: normal;
  }
  .page, .page-group{
    display: block !important;
  }
  .content-block{
    padding: 0;
  }
  ul {
    margin: 0;
    padding: 0;
  }

</style>
