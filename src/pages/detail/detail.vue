<template>
  <div class="">
    <DetailBook />
  </div>
</template>

<script>
  import DetailBook from '../../components/detail/DetailBook'
  import {bookDetail} from '../../api'
  import {getStorageSync} from '../../api/wechat'
  export default {
    components: {DetailBook},
    mounted () {
      const openId = getStorageSync('openId')
      const {fileName} = this.$route.query
      console.log('detail ', openId, fileName)
      if (openId && fileName) {
        bookDetail({openId, fileName}).then(response => {
          console.log(response)
        })
      }
      bookDetail()
    }
  }
</script>

<style lang="scss" scoped>

</style>
