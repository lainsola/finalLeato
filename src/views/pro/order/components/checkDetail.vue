<template> 
  <el-dialog title="查看待审核厂家详情"
             :visible.sync="visible"
             :before-close="handleClose"
             width="40%">
    <font size="4">编号：26<br>
      厂家名称：华为<br>
      企业地址：中国<br>
      法人名称：任正非<br>
      联系电话：6946605<br>
      银行类型：中国银行<br>
      银行对公账户：439061104357<br>
      身份证复印件：<a href="#">点击查看附件内容</a><br>
      企业营业执照：<a href="#">点击查看附件内容</a><br></font>
  </el-dialog>
</template>
<script>
  export default {
    name:'checkDetail',
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
<style></style>


