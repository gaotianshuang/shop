<template>
  <div>
    <!--<yd-navbar>-->
      <img :src="url" alt="彩田学校" slot="center" class="back">
    <!--</yd-navbar>-->
    <yd-button size="large" class="position">{{position}}，你好！</yd-button>
  </div>
</template>
<script>
  export default{
    name: 'PicNight',
    props: ['position', 'url']
  }
</script>
<style>
  .position{
    margin-top: -0.05rem !important;
    background-color: #0399BA !important;
    border-radius: 0 !important;
  }
  .back {
    width: 100%;
    height: 100%;
  }
</style>
