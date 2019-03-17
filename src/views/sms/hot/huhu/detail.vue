<template> 
  <el-dialog title="产品信息查询"
             :visible.sync="visible"
             :before-close="handleClose"
             width="46%">
<hr>
    <img  style="margin-left:25%" src="https://img30.360buyimg.com/n7/s230x322_jfs/t1/16403/11/7328/325973/5c6b6e55E4488160e/1c5534b002d3a9e8.jpg!cc_230x322.jpg " >
    <table class="demo">
    <tr>价格：98rmb</tr>
    <tr>类别：服装饰品</tr>
    <tr>库存：158745件</tr>
    <tr>销量：268  (截止2018-09-15 12:24:27)    </tr>
    <tr>商品毛重：1.0kg</tr>
    <tr>规格尺码：165/84A/S 170/88A/M 175/92A/L 180/96A/XL 185/100A/XXL 190/104A/XXXL</tr>
    <tr>商品编号：41429017488</tr>
    <tr>申请时间：2018-09-15 12:24:27</tr>
    <tr>店铺名：海澜之家</tr>
    <tr>所属厂家：海澜之家股份有限公司</tr>
    <tr>发货地址：中国江苏省江阴市海澜之家大厦</tr>
    </table>
    <hr>
    <p style="font-size: 26px;margin-left: 5%">产品合格证书：</p><br>

<img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1552745643397&di=aa81d5f25f76bf6a7a533c0bbd523192&imgtype=0&src=http%3A%2F%2Fimage6.huangye88.com%2F2014%2F11%2F18%2Fcb165cd35a512e4d.gif" style="margin-left: 20%">
  </el-dialog>
</template>
<script>
  export default {
    name:'logisticsDialog',
    props: {
      value: Boolean
    },
    computed:{
      visible: {
        get() {
          return this.value;
        },
        set(visible){
          this.value=visible;
        }
      }
    },
    data() {
      return {
        logisticsList:Object.assign({},defaultLogisticsList)
      }
    },
    methods:{
      emitInput(val) {
        this.$emit('input', val)
      },
      handleClose(){
        this.emitInput(false);
      }
    }
  }

</script>
<style>
.demo{
font-size: 20px;
  width: 480px;
  margin-left: 10%;
}
</style>

