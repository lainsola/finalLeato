<template> 
  <el-dialog title="丽人丽妆信息查询"
             :visible.sync="visible"
             :before-close="handleClose"
             width="40%">
    <hr>
    <img src="http://imgs.ebrun.com/resources/2017_05/2017_05_12/201705123661494559541605.jpg "  class="huhuhu">
    <br><h2>联系人：陈明雯<br>
    董事长：陈明雯<br>
    联系电话：13945872574<br></h2>
    <br>
    <br>
    厂家名称：丽人丽妆<br>
    厂家地址：上海市松江区乐都西路825弄89<br>
    经营范围：香水、化妆品、日用品、机械设备、五金交电、计算机软硬件（除计算机信息系统安全专用产品）、电子产品、美容工具、服装鞋帽、玩具、针纺织品、母婴产品、文具用品、体育用品、工艺礼品、建筑装潢材料（除危险品）、化工原料.<br>
    联系电话：400886888<br>
    申请时间：2018-09-15 12:24:27<br>
    介绍：丽人丽妆与佳丽宝、爱茉莉太平洋、汉高等国际十大化妆品集团授权合作，获得包括希思黎、奥伦纳素、雪花秀、兰芝、施华蔻等五十多个全球知名品牌在中国的正品授权，成为品牌坚实的合作伙伴。
    <hr>
    <img src="http://img.jdzj.com/UserDocument/2014b/llsxh184/Picture/201451013312.jpg" class="hahaha">
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
  .huhuhu{
    float:left;
    margin-left: 10%;
    left: 50%;
    width:30%;
    height: 30%;
  }
  .hahaha{
    width: 500px;
    height: 400px;
  }
</style>


