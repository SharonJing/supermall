<template>
  <div id="home">
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>
    <scroll class="content" 
    ref="scroll" 
    :probeType='3' 
    :pull-up-load='true'
    @scroll="contentScroll"
    @pullingUp="loadMore">
    <home-swiper :banners='banners' />
    <recommend-view :recommends='recommends'/>
    <feature/>
    <tab-control :title="['流行','新款','精选']" @tabClick='tabClick'/>
    <goods-list :goods='goods[currentType].list'/>
    </scroll>
    <back-top @click.native="backClick" v-show="isBackTop"/>
  </div>
</template>

<script>
import NavBar from 'components/common/navbar/NavBar'
import HomeSwiper from './homecomponents/HomeSwiper'
import RecommendView from './homecomponents/RecommendView'
import Feature from './homecomponents/Feature'
import TabControl from 'components/content/tabControl/TabControl'
import GoodsList from 'components/content/goods/GoodsList'
import Scroll from 'components/common/scroll/Scroll'
import BackTop from 'components/content/backTop/BackTop'

import {getHomeMultidata,getHomeGoods} from 'network/home'

import {debounce} from 'common/utils'
export default {
  data() {
    return {
      banners:[],
      recommends:[],
      goods:{
        'pop':{page:0,list:[]},
        'new':{page:0,list:[]},
        'sell':{page:0,list:[]}
        },
      currentType:'pop',
      isBackTop:false
    }
  },
  components:{
    NavBar,
    HomeSwiper,
    RecommendView,
    TabControl,
    Feature,
    GoodsList,
    Scroll,
    BackTop
  },
  created() {
    this.getHomeMultidata()
    this.getHomeGoods('pop')
    this.getHomeGoods('new')
    this.getHomeGoods('sell')

    
  },
  mounted() {
    const refresh= debounce(this.$refs.scroll.refresh,200)
    this.$bus.$on('itemImageLoad',()=>{
      refresh()
    })
  },
  methods: {
    tabClick(index){
      switch (index) {
        case 0:
          this.currentType='pop'
          break;
        case 1:
          this.currentType='new'
          break;
        case 2:
          this.currentType='sell'
          break;
      }
    },
    backClick(){
      this.$refs.scroll.backTop(0, 0)
    },
    contentScroll(position){
      this.isBackTop= (-position.y)>1000
    },
    loadMore(){
      this.getHomeGoods(this.currentType)
    },
   
    // 网络请求的方法
    getHomeMultidata(){
      getHomeMultidata().then(res=>{
      console.log(res);
      this.banners=res.data.banner.list
      this.recommends =res.data.recommend.list
    })
    },
    getHomeGoods(type){
      const page = this.goods[type].page + 1
      getHomeGoods(type, page).then(res=>{
        this.goods[type].list.push(...res.data.list)
        this.goods[type].page += 1
        // 完成了下拉加载更多
        this.$refs.scroll.finishPullUp()
        // console.log(res);
    })
    }
  },
}
</script>

<style scoped>
#home{
  padding-top: 44px;
  position: relative;
  height: 100vh;
}
.home-nav {
  background-color: var(--color-tint);
  color: #fff;

  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 9;
}
.content{
  position: absolute;
  top: 44px;
  bottom: 49px;
  left: 0;
  right: 0;
}
</style>