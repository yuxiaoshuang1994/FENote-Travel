<template>
  <div>
    <home-hearder/>
    <home-swiper :swiperList="swiperList"/>
    <home-icons :icons="icons"/>
    <home-recommend :recommendList="recommendList"/>
    <home-weekend :weekendList="weekendList"/>
  </div>
</template>

<script>
  import HomeHearder from './components/Hearder'
  import HomeSwiper from './components/Swiper'
  import HomeIcons from './components/Icons'
  import HomeRecommend from './components/Recommend'
  import HomeWeekend from './components/Weekend'
  import axios from 'axios'
  import { mapState } from 'vuex'

  export default {
    name: 'Home',
    components: {
      HomeHearder,
      HomeSwiper,
      HomeIcons,
      HomeRecommend,
      HomeWeekend
    },
    data () {
      return {
        swiperList: [],
        icons: [],
        recommendList: [],
        weekendList: [],
        lastCity: ''
      }
    },
    mounted () {
      this.getHomeInfo()
      this.lastCity = this.city
    },
    // 当使用 keep-alive 时，会多出这个生命周期方法
    activated () {
      // 当页面重新显示时，被执行
      if (this.lastCity !== this.city) {
        this.lastCity = this.city
        this.getHomeInfo()
      }
    },
    computed: {
      ...mapState(['city'])
    },
    methods: {
      getHomeInfo () {
        axios.get('https://raw.githubusercontent.com/yuxiaoshuang1994/FENote-Travel/master/static/mock/index.json?city=' + this.city).then(this.getHomeInfoSucc).catch(function (error) {
          console.log(error)
        })
      },
      getHomeInfoSucc (res) {
        res = res.data
        if (res.ret && res.data) {
          let data = res.data
          this.swiperList = data.swiperList
          this.icons = data.iconList
          this.recommendList = data.recommendList
          this.weekendList = data.weekendList
        }
      }
    }
  }
</script>

<style scoped>

</style>
