<template>
  <div class="auth-wrapper">
    <div class="auth-inner-wrapper">
      <div class="auth-info-wrapper">
        <div class="auth-img">
          <ImageView
            src="https://i.bmp.ovh/imgs/2021/05/8d77a01b1b73e25b.png"
            height="100%"
            mode="scaleToFill"
            round
          />
        </div>
        <div class="auth-title">登录闪电读书</div>
        <div class="auth-subtitle">全球好书免费读</div>
      </div>
      <button
        class="auth-button"
        open-type="getUserInfo"
        @getuserinfo="getUserInfo"
      >授权登录</button>
    </div>
  </div>
</template>

<script>
  import ImageView from './ImageView'
  export default {
    components: {ImageView},
    methods: {
      getUserInfo () {
        this.$emit('getUserInfo')
      }
    }
  }
</script>

<style lang="scss" scoped>
.auth-wrapper{
  position: fixed;
  left: 0;
  top: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
  background: #999;
  z-index: 1000;
  .auth-inner-wrapper{
    position: relative;
    width: 270px;
    height: 248px;
    background: #fff;
    border-radius: 18px;
    .auth-info-wrapper{
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      margin-top: 22.5px;
      .auth-img{
        width: 95px;
        height: 100px;
      }
      .auth-title{
        font-size: 13px;
        color: #999;
        line-height: 18.5px;
        margin-top: 8px;
      }
      .auth-subtitle{
        font-size: 16.5px;
        color: #333;
        line-height: 22.5px;
        font-weight: 400;
        margin-top: 3.5px;
      }
    }
    .auth-button{
      position: absolute;
      bottom: 0;
      width: 100%;
      font-size: 15px;
      color: #fff;
      height: 49px;
      line-height: 49px;
      background-image: linear-gradient(90deg,#1ea3f5 0%, #0c68c2 100%);
      border-radius: 0 0 18px 18px;
    }
  }
}
</style>
