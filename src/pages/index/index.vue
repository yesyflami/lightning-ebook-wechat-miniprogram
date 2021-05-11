<template>
  <div>
    <div class="home" v-if="isAuth">
      <SearchBar
        disabled
        @onClick="onSearchBarClick"
        :hot-search="hotSearch"
      />
      <HomeCard
        :data="homeCard"
        @onClick="onHomeBookClick"
      />
      <HomeBanner
        img="http://www.youbaobao.xyz/book/res/bg.jpg"
        title="4·23图书节 用心磨字，以心博爱"
        subTitle="阅读让生活更精彩"
        @onClick="onBannerClick"
      />
      <div :style="{marginTop: '23px'}">
        <HomeBook
          title="为你推荐"
          :row="1"
          :col="3"
          :data="recommend"
          mode="col"
          btn-text="换一批"
          @onMoreClick="recommendChange('recommend')"
          @onBookClick="onHomeBookClick"
        />
      </div>
      <div :style="{marginTop: '23px'}">
        <HomeBook
          title="免费阅读"
          :row="2"
          :col="2"
          :data="freeRead"
          mode="row"
          btn-text="换一批"
          @onMoreClick="recommendChange('freeRead')"
          @onBookClick="onHomeBookClick"
        />
      </div>
      <div :style="{marginTop: '23px'}">
        <HomeBook
          title="当前最热"
          :row="1"
          :col="4"
          :data="hotBook"
          mode="col"
          btn-text="换一批"
          @onMoreClick="recommendChange('hotBook')"
          @onBookClick="onHomeBookClick"
        />
      </div>
      <div :style="{marginTop: '23px'}">
        <HomeBook
          title="分类"
          :row="3"
          :col="2"
          :data="category"
          mode="category"
          btn-text="查看全部"
          @onMoreClick="onCategoryMoreClick"
          @onBookClick="onHomeBookClick"
        />
      </div>
    </div>
    <Auth v-if="!isAuth" @getUserInfo="init"/>
  </div>
</template>

<script>
  import SearchBar from '../../components/home/searchBar'
  import HomeCard from '../../components/home/HomeCard'
  import HomeBanner from '../../components/home/HomeBanner'
  import HomeBook from '../../components/home/HomeBook'
  import Auth from '../../components/base/Auth'
  import {getHomeData, recommend, freeRead, hotBook, register} from '../../api'
  import {
    getSetting,
    getUserInfo,
    setStorageSync,
    getStorageSync,
    getUserOpenId,
    showLoading,
    hideLoading
  } from '../../api/wechat'

  export default {
    components: {
      HomeBook,
      HomeBanner,
      HomeCard,
      SearchBar,
      Auth
    },
    data () {
      return {
        hotSearch: '',
        homeCard: {},
        shelf: [],
        banner: {},
        recommend: [],
        freeRead: [],
        hotBook: [],
        category: [],
        isAuth: false
      }
    },
    mounted () {
      this.init()
    },
    methods: {
      recommendChange (key) {
        switch (key) {
          case 'recommend':
            recommend().then(response => {
              this.recommend = response.data.data
            })
            break
          case 'freeRead':
            freeRead().then(response => {
              this.freeRead = response.data.data
            })
            break
          case 'hotBook':
            hotBook().then(response => {
              this.hotBook = response.data.data
            })
            break
        }
      },
      onSearchBarClick () {
        this.$router.push({
          path: '/pages/search/main',
          query: {
            hotSearch: this.hotSearch
          }
        })
      },
      onBannerClick () {
        console.log('Banner Click!')
      },
      onCategoryMoreClick () {
        this.$router.push({
          path: '/pages/categoryList/main'
        })
      },
      onHomeBookClick (book) {
        console.log(book)
        this.$router.push({
          path: '/pages/detail/main',
          query: {
            fileName: book.fileName
          }
        })
      },
      getHomeData (openId, userInfo) {
        getHomeData({openId}).then(response => {
          const {
            data: {
              hotSearch: {
                keyword
              },
              shelf,
              banner,
              recommend,
              freeRead,
              hotBook,
              category,
              shelfCount
            }
          } = response.data
          this.hotSearch = keyword
          this.banner = banner
          this.recommend = recommend
          this.freeRead = freeRead
          this.hotBook = hotBook
          this.category = category
          this.homeCard = {
            bookList: shelf,
            num: shelfCount,
            userInfo
          }
          hideLoading()
        }).catch(() => {
          hideLoading()
        })
      },
      getUserInfo () {
        const onOpenIdComplete = (openId, userInfo) => {
          this.getHomeData(openId, userInfo)
          register(openId, userInfo)
        }
        getUserInfo(
          (userInfo) => {
            console.log(userInfo)
            setStorageSync('userInfo', userInfo)
            const openId = getStorageSync('openId')
            if (!openId || openId.length === 0) {
              getUserOpenId(openId => onOpenIdComplete(openId, userInfo))
            } else {
              onOpenIdComplete(openId, userInfo)
            }
          },
          () => {
            console.log('failed..') // 获取用户信息，抛出异常
          }
        )
      },
      getSetting () {
        getSetting('userInfo', () => {
          console.log('成功')
          this.isAuth = true
          showLoading('正在加载')
          this.getUserInfo()
        }, () => {
          console.log('失败')
          this.isAuth = false
        })
      },
      init () {
        this.getSetting()
      }
    }
  }
</script>

<style lang="scss" scoped>

</style>
