<template>
  <section class="msite">
    <!--首页头部-->
      <Header :title="address.name || '正在定位中。。。'">
          <span class="header_search" slot="left">
            <i class="iconfont icon-sousuo"></i>
          </span>
          <span class="header_login" slot="right">
            <span class="header_login_text">登录|注册</span>
          </span>
      </Header>
    <!--首页导航-->
    <nav class="msite_nav border-1px">
      <div class="swiper-container">
        <div class="swiper-wrapper">
          <!-- 写一个二维数组categorysArr -->
          <div class="swiper-slide" v-for="(categorys,index) in categorysArr" :key="index">
            <a href="javascript:" class="link_to_food" v-for="(c,index) in categorys" :key="index">
              <div class="food_container">
                <img :src="'https://fuss10.elemecdn.com' +c.image_url">
              </div>
              <span>{{c.title}}</span>
            </a>

          </div>
        </div>
        <!-- Add Pagination -->
        <div class="swiper-pagination"></div>
      </div>
    </nav>

    <ShopList/>
  </section>
</template>
<script>
import Swiper from 'swiper'
import "swiper/dist/css/swiper.css"
import {mapState} from 'vuex'
import ShopList from '../../components/ShopList/ShopList'


export default {
  computed:{
    ...mapState(['address','categorys']),
  /* 根据分类的一维数组生成二维数组 */
   /*
      根据分类的一维数组生成二维数组
      小数组的最大长度为8
      */
      categorysArr () {
        // 取出相关的数据
        const bigArr = []
        let smallArr = []
        const {categorys} = this
        // 计算产生结果
        categorys.forEach(c => {
          // 将小数组放入大数组(同一个小数组只能被保存一次)
          if (smallArr.length===0) {
            bigArr.push(smallArr)
          }
          // 将分类对象放入小数组(小数组的长度最大为8)
          smallArr.push(c)
          // 如果满了, 重新准备一个新的小数组
          if (smallArr.length===8) {
            smallArr = []
          }

        })
        // 返回结果
        return bigArr
      }


  },
  watch:{
    categorys(){
          this.$nextTick(()=>{
               new Swiper ('.swiper-container', {
            /*  direction: 'vertical', // 垂直切换选项 */
              loop: true, // 循环模式选项

              // 如果需要分页器
              pagination: {
                el: '.swiper-pagination',
              },

              /* // 如果需要前进后退按钮
              navigation: {
                nextEl: '.swiper-button-next',
                prevEl: '.swiper-button-prev',
              },

              // 如果需要滚动条
              scrollbar: {
                el: '.swiper-scrollbar',
              }, */
            })
          })

    }
  },
  /* mounted组件界面初始化之后立即执行 */
  mounted(){
    /* 分发action，异步获取商家列表 */
    this.$store.dispatch('getShops')
      /* 分发action，异步获取分类列表 */
    this.$store.dispatch('getCategorys')

  },
  components:{
    ShopList
  }


}
</script>
<style lang="stylus" rel="stylesheet/stylus">
  @import '../../commont/stylus/mixins.styl'
  .msite  //首页
    width 100%

    .msite_nav
      bottom-border-1px(#e4e4e4)
      margin-top 45px
      height 200px
      background #fff
      .swiper-container
        width 100%
        height 100%
        .swiper-wrapper
          width 100%
          height 100%
          .swiper-slide
            display flex
            justify-content center
            align-items flex-start
            flex-wrap wrap
            .link_to_food
              width 25%
              .food_container
                display block
                width 100%
                text-align center
                padding-bottom 10px
                font-size 0
                img
                  display inline-block
                  width 50px
                  height 50px
              span
                display block
                width 100%
                text-align center
                font-size 13px
                color #666
        .swiper-pagination
          >span.swiper-pagination-bullet-active
            background #02a774


</style>
