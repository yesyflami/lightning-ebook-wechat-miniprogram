<template>
  <div>
    <HomeBook
      :row="Math.floor(category.length / 2 )"
      :col="2"
      :data="category"
      mode="category"
      :show-btn="false"
      :show-title="false"
    />
  </div>
</template>

<script>
  import HomeBook from '../../components/home/HomeBook'
  import {categoryList} from '../../api'
  export default {
    components: {HomeBook},
    data () {
      return {
        category: []
      }
    },
    mounted () {
      categoryList().then(response => {
        this.category = response.data.data
      })
    }
  }
</script>

<style lang="scss" scoped>

</style>
