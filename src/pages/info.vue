<template>
  <div class="info-page">
    <div class="info-item">
      <img src="@/assets/info/1.png" alt="" class="info-item-img">
      <p class="info-item-title">帐户余额<span style="margin-left: 14px;">{{balance}}</span></p>
      <div class="btn-group">
        <div class="btn">充值</div>
        <div class="btns">提现</div>
      </div>
    </div>
    <div class="info-item">
      <img src="@/assets/info/2.png" alt="" class="info-item-img">
      <p class="info-item-title">推广收益<span style="margin-left: 14px">{{balanceAll}}</span></p>
      <div class="btn-group">
        <div class="btn">转到余额</div>
      </div>
    </div>
    <div class="info-card">
      <div class="info-title">
          最近订单
          <router-link to="/order" class="info-title-a">查看所有订单</router-link>
      </div>
      <div class="info-content">
          <template  v-if="list.length">
            <Order v-for="(item, index) in list" :key="index"
              :Lists="item.orderFormProductList"
              :amountFinally = "item.amountFinally"
              :datelineCreateReadable = "item.datelineCreateReadable"
              :orderId = "item.serialNum"
              :id = "item.id"
              :statusText = "item.statusText"
              :paid = "item.paid"
              :refund = "item.refund"
              :reviews = "item.reviews"
              :done = "item.done"
              :delivery = "item.delivery"
            />
          </template>
          <template v-else> 
            <img src="@/assets/info/3.png" alt="" width="120px">
            <p>您最近没有待处理订单</p>
          </template>
      </div>
    </div>
  </div>
</template>
<script>
import Order from '@/components/order'
  import { mapState, mapGetters } from 'vuex' 
  export default {
    data() {
      return {
        activeName: 'first',
        profit:1
      };
    },
    computed:{
      ...mapState({
        balance: state => state.user.balance,
        balanceAll: state => state.commission.balanceAll
      }),
      ...mapGetters({
        list: 'orderList'
      })
    },
    components:{
      Order
    },
    watch:{
      list(){
        console.log(this.list)
      }
    },
    created(){
      this.$store.dispatch('user/balance')
      this.$store.dispatch('commission/detail')
      this.$store.dispatch('cart/info')
      this.$store.dispatch('order/list', {offset:0, limit: 3})
    },
    methods: {
      handleClick(tab, event) {
        console.log(tab, event);
      }
    }
  };
</script>

<style scoped>
.info-page{
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  background: #F8F8F8;
}
.info-item{
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 49%;
  height: 120px;
  background: #fff;
  padding: 0px 40px;
  box-sizing: border-box;
}

.info-item-img{
  width: 26px;
  height: 26px;
}
.info-item-title{
  width: 100%;
  margin-left: 20px;
  text-align: left;   
  font-size: 14px;
  font-family: PingFang SC;
  font-weight: 400;
  line-height: 29px;
  color: #1B1B1B;
}
.btn-group{
  min-width: 160px;
  display: flex;
  align-items: center;
}
.info-card{
    background: #fff;
    width: 100%;
    /* height: 200px; */
    padding: 30px 0px;
    margin-top: 20px;
}
.btn{
  height: 32px;
  padding: 0px 16px;
  background: #014785;
  border: 1px solid #014785;
  display: flex;
  justify-content: center;
  align-items: center;
  color:#fff;
  margin-right: 10px;
  cursor: pointer;
}
.btns{
  height: 32px;
  padding: 0px 16px;
  background: #fff;
  border: 1px solid #014785;
  display: flex;
  justify-content: center;
  align-items: center;
  color:#014785;
  cursor: pointer;
}
.info-title{
    /* font-size: 24px;
    font-weight: 600;
     */
    font-size: 22px;
    font-family: PingFang SC;
    font-weight: bold;
    line-height: 40px;
    color: #014785;
    padding-left: 60px;
    position: relative;
}
.info-title:before{
    content: '';
    width: 4px;
    height: 34px;
    position: absolute;
    top: 4px;
    background: #014785;
    left: 44px;

}
.info-content{
    display: flex;
    flex-direction: column;
    margin-left: 40px;
    margin-top: 40px;
    justify-content:  flex-start;
    align-items: center;
    /* flex-wrap: wrap; */
}
.content{
    margin:0px 40px;
}
p {
  font-size: 12px;
  font-family: PingFang SC;
  font-weight: 400;
  line-height: 29px;
  color: #5F5F5F;
}
.info-title-a{
  font-size: 12px;
  color:#0082FF;
  text-decoration: none;
  float: right;
  margin-right: 20px;
}
</style>