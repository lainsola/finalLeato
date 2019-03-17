<template> 
  <el-dialog title="华为信息查询"
             :visible.sync="visible"
             :before-close="handleClose"
             width="40%">
<hr>
    <img src="http://itsummit.sz.gov.cn/szjs/qyjs/201102/W020110219606005274847.jpg "  class="huhuhu">
    <br><h2>联系人：郭平<br>
    董事长：梁华<br>
    联系电话：13945872574<br></h2>
    <br>
    厂家名称：华为<br>
    厂家地址：中国广东省深圳市龙岗区坂田街道华为基地<br>
    经营范围：IT、无线电，微电子，通讯，路由，交换等<br>
    联系电话：400886888<br>
    申请时间：2018-09-15 12:24:27<br>
    介绍：华为是全球领先的ICT（信息与通信）基础设施和智能终端提供商，致力于把数字世界带入每个人、每个家庭、每个组织，构建万物互联的智能世界。我们在通信网络、IT、智能终端和云服务等领域为客户提供有竞争力、安全可信赖的产品、解决方案与服务，与生态伙伴开放合作，持续为客户创造价值，释放个人潜能，丰富家庭生活，激发组织创新。华为坚持围绕客户需求持续创新，加大基础研究投入，厚积薄发，推动世界进步。华为成立于1987年，是一家由员工持有全部股份的民营企业，目前有18万员工，业务遍及170多个国家和地区。<br
    <hr>
<img src="http://img3.imgtn.bdimg.com/it/u=3577098201,3625016662&fm=26&gp=0.jpg">
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
</style>


