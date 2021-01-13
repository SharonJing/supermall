<template>
  <div id="home">
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>
    <home-swiper :banners='banners' />
    <recommend-view :recommends='recommends'/>
    <feature/>
    <tab-control class="tab-control" :title="['流行','新款','精选']"/>
    <ul>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
    </ul>
  </div>
</template>

<script>
import NavBar from 'components/common/navbar/NavBar'
import HomeSwiper from './homecomponents/HomeSwiper'
import RecommendView from './homecomponents/RecommendView'
import Feature from './homecomponents/Feature'
import TabControl from 'components/content/tabControl/TabControl'
import {getHomeMultidata} from 'network/home'
export default {
  data() {
    return {
      banners:[],
      recommends:[]
    }
  },
  components:{
    NavBar,
    HomeSwiper,
    RecommendView,
    TabControl,
    Feature
  },
  created() {
    getHomeMultidata().then(res=>{
      console.log(res);
      this.banners=res.data.banner.list
      this.recommends =res.data.recommend.list
    })
  },
}
</script>

<style scoped>
.home-nav {
  background-color: var(--color-tint);
  color: #fff;
}
.tab-control{
  position: sticky;
  top: 44px;
}
</style>