<template>
  <div class="pos">
    <div>
      <el-row >
        <el-col :span='7' id="order-list">
          我是订单栏
          <el-tabs>
            <el-tab-pane label="点餐">
              <el-table :data="tableData" border show-summary style="width: 100%" >
                <el-table-column prop="goodsName" label="商品"  ></el-table-column>
                <el-table-column prop="count" label="量" width="50"></el-table-column>
                <el-table-column prop="price" label="金额" width="70"></el-table-column>
                <el-table-column  label="操作" width="100" fixed="right">
                  <template scope="scope">
                    <el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
                    <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                  </template>
                </el-table-column>
              </el-table>

              <el-button type='warning'>挂单</el-button>
              <el-button type='danger' @click="delAllGoods()">删除</el-button>
              <el-button type='scucess' @click="checkout()">结账</el-button>
            </el-tab-pane>

            <el-tab-pane label="挂单">
              挂单
            </el-tab-pane>
            <el-tab-pane label="外卖">
              外卖
            </el-tab-pane>
          </el-tabs>
        </el-col>

        <!--商品展示-->
        <el-col :span="17">
          <div class="often-goods">
            <div class="title">常用商品</div>
            <div class="often-goods-list">
              <ul>
                <li v-for="good in oftenGoods" :key="good.goodsId" @click="addOrderList(good)">
                  <span>{{good.goodsName}}</span>
                  <span class="o-price">${{good.price}}元</span>
                </li>
              </ul>
            </div>
          </div>

          <div class="goods-type">
            <el-tabs>
              <el-tab-pane label="汉堡">
                <ul class='cookList'>
                  <li v-for="good in hanbaoGoods" :key="good.goodsId" @click="addOrderList(good)">
                    <span class="foodImg">
                      <img :src="good.goodsImg" width="100%">
                    </span>
                    <span class="foodName">{{good.goodsName}}</span>
                    <span class="foodPrice">￥{{good.price}}元</span>
                  </li>
                </ul>
              </el-tab-pane>

              <el-tab-pane label="饮料">
                <ul class='cookList'>
                  <li v-for="good in yinliaoGoods" :key="good.goodsId" @click="addOrderList(good)">
                    <span class="foodImg">
                      <img :src="good.goodsImg" width="100%">
                    </span>
                    <span class="foodName">{{good.goodsName}}</span>
                    <span class="foodPrice">￥{{good.price}}元</span>
                  </li>
                </ul>
              </el-tab-pane>

              <el-tab-pane label="小吃">
                <ul class='cookList'>
                  <li v-for="good in xiaocheGoods" :key="good.goodsId" @click="addOrderList(good)">
                    <span class="foodImg">
                      <img :src="good.goodsImg" width="100%">
                    </span>
                    <span class="foodName">{{good.goodsName}}</span>
                    <span class="foodPrice">￥{{good.price}}元</span>
                  </li>
                </ul>
              </el-tab-pane>

              <el-tab-pane label="零食">
                <ul class='cookList'>
                  <li v-for="good in linshiGoods" :key="good.goodsId" @click="addOrderList(good)">
                    <span class="foodImg">
                      <img :src="good.goodsImg" width="100%">
                    </span>
                    <span class="foodName">{{good.goodsName}}</span>
                    <span class="foodPrice">￥{{good.price}}元</span>
                  </li>
                </ul>
              </el-tab-pane>
            </el-tabs>
          </div>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script>
import leftNav from '@/components/common/leftNav'
import axios from 'axios'
export default {
  name: 'Pos',
  comments: {
    leftNav
  },
  mounted: function () {
    var orderHeight = document.body.clientHeight
    document.getElementById('order-list').style.height = orderHeight + 'px'
  },

  created () {
    var url1 = 'https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/oftenGoods';
    axios.get(url1)
      .then(response => {
        this.oftenGoods = response.data
      })
      .catch(error => {
        console.log(error)
        alert('网络请求错误')
      })

    var url2 = 'https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/typeGoods'
    axios.get(url2)
      .then(response => {
        this.hanbaoGoods = response.data[0]
        this.xiaocheGoods = response.data[1]
        this.yinliaoGoods = response.data[2]
        this.linshiGoods = response.data[3]
      })
      .catch(error => {
        console.log(error)
        alert('网络请求错误')
      })
  },

  methods: {
    addOrderList (good) {
      this.totalCount = 0
      this.totalMoney = 0
      let isHave = false
      for (let i = 0; i < this.tableData.length; i++) {
        if (this.tableData[i].goodsId === good.goodsId) {
          isHave = true
        }
      }

      // 根据isHave的值判断订单列表中是否已经有此商品
      if (isHave) {
        // 存在就进行数量添加
        let arr = this.tableData.filter(o => o.goodsId === good.goodsId)
        arr[0].count++
      } else {
        let newGoods = {goodsId: good.goodsId, goodsName: good.goodsName, price: good.price, count: 1}
        this.tableData.push(newGoods)
      }

      // 进行数量和价格的汇总计算
      this.tableData.forEach((element) => {
        this.totalCount += element.count
        this.totalMoney = this.totalMoney + (element.price * element.count)
      })
    },

    delSingleGoods (goods) {
      console.log(goods)
      // this.tableData = this.tableData.filter(o => o.goodsId !== goods.goodsId)
      let arr = this.tableData.filter(o => o.goodsId === goods.goodsId)
      arr[0].count--
      this.getAllMoney()
    },

    // 汇总数量和金额
    getAllMoney () {
      this.totalCount = 0
      this.totalMoney = 0
      if (this.tableData) {
        this.tableData.forEach((element) => {
          this.totalCount += element.count
          this.totalMoney = this.totalMoney + (element.price * element.count)
        })
      }
    },

    // 删除所有商品
    delAllGoods () {
      this.tableData = []
      this.totalCount = 0
      this.totalMoney = 0
    },

    checkout () {
      if (this.totalCount !== 0) {
        this.tableData = []
        this.totalCount = 0
        this.totalMoney = 0
        this.$message({
          message: '结账成功，感谢你又为店里出了一份力!',
          type: 'success'
        })
      } else {
        this.$message.error('不能空结。老板了解你急切的心情！')
      }
    }
  },

  data () {
    return {
      tableData: [],
      oftenGoods: [],
      hanbaoGoods: [],
      xiaocheGoods: [],
      yinliaoGoods: [],
      linshiGoods: [],
      totalCount: 0,
      totalMoney: 0
    }
  }
}
</script>

<style scoped>
  .pos {
    float: left;
    width: 95%;
    background-color:#EFF2F7;
    height: 100%;
    overflow: auto;
  }

  .title {
    height: 20px;
    border-bottom: 1px solid #D3DCE6;
    background-color: #F9FAFC;
    padding: 10px;
  }

  .often-goods-list ul li {
    list-style: none;
    float: left;
    border: 1px solid #E5E9F2;
    padding: 10px;
    margin: 5px;
    background-color: #FFF;
  }

  .o-price{
    color:darkred;
  }

  .cookList li{
    list-style: none;
    width:23%;
    border:1px solid #E5E9F2;
    height: auto;
    overflow: hidden;
    background-color:#fff;
    padding: 2px;
    float:left;
    margin: 2px;
  }

  .cookList li span{
    display: block;
    float:left;
  }

  .foodImg{
    width: 40%;
  }

  .foodName{
    font-size: 18px;
    padding-left: 10px;
    color:brown;
  }

  .foodPrice{
    font-size: 16px;
    padding-left: 10px;
    padding-top:10px;
  }

  .goods-type {
    clear: both;
  }

  .o-price {
    color: #58B7FF;
  }

  .often-goods-list {
    border-bottom: 1px solid #C0CCDA;
    height: auto;
    overflow: hidden;
    padding-bottom: 10px;
    background-color: #F9FAFC;
  }

  .cookList li {
    list-style: none;
    width: 23%;
    border: 1px solid #E5E9F2;
    height: auto;
    overflow: hidden;
    background-color: #fff;
    padding: 2px;
    float: left;
    margin: 2px;
    cursor: pointer;
  }

  .cookList li span {

    display: block;
    float: left;
  }

  .foodImg {
    width: 40%;
  }

  .foodName {
    font-size: 18px;
    padding-left: 10px;
    color: brown;
  }

  .foodPrice {
    font-size: 16px;
    padding-left: 10px;
    padding-top: 10px;
  }

  .totalDiv {
    height: auto;
    overflow: hidden;
    text-align: right;
    font-size: 16px;
    background-color: #fff;
    border-bottom: 1px solid #E5E9F2;
    padding: 10px;
  }
</style>
