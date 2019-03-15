<template> 
  <el-dialog title="LG信息查询"
             :visible.sync="visible"
             :before-close="handleClose"
             width="40%">
    <hr>
    <img src="http://technuter.com/wp-content/uploads/2014/02/LG-Electronics-logo.jpg "  class="huhuhu">
    <br><h2>联系人：具本茂<br>
    董事长：具本茂<br>
    联系电话：13945872574<br></h2>
    <br>
    厂家名称：LG集团<br>
    厂家地址：韩国首尔市永登浦区汝矣岛洞20号 LG双子座大厦<br>
    经营范围：化学能源、电子电器、通讯与服务等领域<br>
    联系电话：13598423454<br>
    申请时间：2018-09-15 12:24:27<br>
    介绍：韩国LG集团于1947年成立于韩国首尔，位于首尔市永登浦区汝矣岛洞20号。是领导世界产业发展的国际性企业集团。LG集团目前在171个国家与地区建立了300多家海外办事机构。旗下子公司有：LG电子、LG display、GS加德士、LG化学、LG生活健康等，事业领域覆盖化学能源、电子电器、通讯与服务等领域。
    <hr>
    <img src="http://img1.efu.com.cn/upfile/corp/2014-09/352d0ffc-7381-4e58-ab0f-b0bb1cb7f7e9.jpg" class="kaka">
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
  .kaka{
    width: 400px;
    height: 650px;
  }
</style>


