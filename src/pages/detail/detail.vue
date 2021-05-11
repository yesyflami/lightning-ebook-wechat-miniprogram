<template>
  <div class="">
    <DetailBook :book="book"/>
    <DetailStat
      :readers="book.readers"
      :readerNum="book.readNum"
      :rankNum="book.rankNum"
      :rankAvg="book.rankAvg"
    />
    <DetailRate
      :rate-value="book.rateValue"
      @onRateChange="onRateChange"
    />
    <DetailContents
      :contents="contents"
      @readBook="readBook"
    />
    <DetailBottom

    />
  </div>
</template>

<script>
  import {bookContents, bookDetail, bookIsInShelf, bookRankSave} from '../../api'
  import {getStorageSync} from '../../api/wechat'
  import DetailBook from '../../components/detail/DetailBook'
  import DetailStat from '../../components/detail/DetailStat'
  import DetailRate from '../../components/detail/DetailRate'
  import DetailContents from '../../components/detail/DetailContents'
  import DetailBottom from '../../components/detail/DetailBottom'

  export default {
    components: {DetailBottom, DetailContents, DetailRate, DetailStat, DetailBook},
    data () {
      return {
        book: {},
        contents: []
      }
    },
    methods: {
      readBook (href) {
        console.log(href)
      },
      onRateChange (value) {
        const openId = getStorageSync('openId')
        const {fileName} = this.$route.query
        bookRankSave({
          openId,
          fileName,
          rank: value
        }).then(response => {
          this.getBookDetail()
        })
      },
      getBookDetail () {
        const openId = getStorageSync('openId')
        const {fileName} = this.$route.query
        if (openId && fileName) {
          bookDetail({openId, fileName}).then(response => {
            this.book = response.data.data
          })
        }
      },
      getBookContents () {
        const {fileName} = this.$route.query
        if (fileName) {
          bookContents({fileName}).then(response => {
            this.contents = response.data.data
          })
        }
      },
      getBookIsInShelf () {
        const openId = getStorageSync('openId')
        const {fileName} = this.$route.query
        if (openId && fileName) {
          bookIsInShelf({openId, fileName}).then(response => {})
        }
      }
    },
    mounted () {
      this.getBookDetail()
      this.getBookContents()
      this.getBookIsInShelf()
    }
  }
</script>

<style lang="scss" scoped>

</style>
