<template>
  <div class="search-bar">
    <div class="search-bar-wrapper" @click="onSearchBarClick">
      <van-icon
        name="search"
        class="search"
      />
      <input
        type="text"
        class="search-bar-input"
        :focus="focus"
        :disabled="disabled"
        :maxlength="limit"
        placeholder="搜索"
        v-model="searchWord"
        @input="onChange"
        confirm-type="search"
        @confirm="onConfirm"
        placeholder-style="color: #adb4be"
      >
      <van-icon
        name="clear"
        class="clear"
        @click="onClearClick"
        v-if="searchWord.length > 0"
      />
    </div>
  </div>
</template>

<script>
  export default {
    props: {
      focus: {
        type: Boolean,
        default: false
      },
      disabled: {
        type: Boolean,
        default: false
      },
      limit: {
        type: Number,
        default: 50
      },
      hotSearch: {
        type: String,
        default: ''
      }
    },
    data () {
      return {
        searchWord: ''
      }
    },
    methods: {
      onSearchBarClick () {
        this.$emit('onClick')
      },
      onClearClick () {
        this.searchWord = ''
        this.$emit('onClear')
      },
      onChange (e) {
        const {value} = e.mp.detail
        this.$emit('onChange', value)
      },
      onConfirm (e) {
        const {value} = e.mp.detail
        this.$emit('onConfirm', value)
      },
      setValue (v) {
        this.searchWord = v
      },
      getValue () {
        return this.searchWord
      }
    }
  }
</script>

<style lang="scss" scoped>
.search-bar{
  padding: 15px 15.5px;
  .search-bar-wrapper{
    display: flex;
    align-items: center;
    height: 42px;
    box-sizing: border-box;
    background: #f5f7f9;
    border-radius: 20px;
    padding: 12px 17px;
    color: #6e757c;
    .search-bar-input{
      flex: 1;
      margin: 0 8px;
      color: #333;
      font-size: 15px;
    }
    .search,.clear{
      display: flex;
      align-items: center;
      height: 100%;
    }
  }
}
</style>
