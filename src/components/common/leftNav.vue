<template>
  <div class="left-nav">
    <ul>
      <li>
        <i class="icon iconfont icon-shouyin"></i>
        <div>收银</div>
      </li>

      <li>
        <i class="icon iconfont icon-dianpu"></i>
        <div>店铺</div>
      </li>

      <li>
        <i class="icon iconfont icon-ziyuan"></i>
        <div>商品</div>
      </li>

      <li>
        <i class="icon iconfont icon-huiyuan"></i>
        <div>会员</div>
      </li>

      <li>
        <i class="icon iconfont icon-tongji"></i>
        <div>统计</div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'leftNav',
  data () {
    return {

    }
  }
}
</script>

<style scoped>
  .left-nav{
    color:#fff;
    font-size:10px;
    height:100%;
    background-color: #8B5F65;
    float:left;
    width: 5%;
  }

  .iconfont{
    font-size:30px;
  }

  .left-nav ul{
    padding:0px;
    margin: 0px;
  }

  .left-nav li{
    list-style: none;
    text-align: center;
    border-bottom:1px solid #FF8C00;
    padding:10px;
  }
</style>
