<template> 
  <div class="app-container">
    <el-card class="filter-container" shadow="never">
      <div>
        <i class="el-icon-search"></i>
        <span>搜索</span>
        <el-button
          style="float:right"
          type="primary"
          @click="handleSearchList()"
          size="small">
          查询搜索
        </el-button>
        <el-button
          style="float:right;margin-right: 15px"
          @click="handleResetSearch()"
          size="small">
          重置
        </el-button>
      </div>
      <div style="margin-top: 15px">
        <el-form :inline="true" :model="listQuery" size="small" label-width="140px">
          <el-form-item label="输入搜索：">
            <el-input v-model="listQuery.id" class="input-width" placeholder="编号"></el-input>
          </el-form-item>
          <el-form-item label="厂商名：">
            <el-input v-model="listQuery.brandName" class="input-width" placeholder="姓名"></el-input>
          </el-form-item>
          <el-form-item label="注册时间：">
            <el-date-picker
              class="input-width"
              v-model="listQuery.createTime"
              value-format="yyyy-MM-dd"
              type="date"
              placeholder="请选择时间">
            </el-date-picker>
          </el-form-item>
        </el-form>
      </div>
    </el-card>
    <el-card class="operate-container" shadow="never">
      <i class="el-icon-tickets"></i>
      <span>数据列表</span>

    </el-card>
    <div class="table-container">
      <el-table ref="orderTable"
                :data="list"
                style="width: 100%;"
                @selection-change="handleSelectionChange"
                v-loading="listLoading" border>
        <el-table-column type="selection" width="60" align="center"></el-table-column>
        <el-table-column label="编号" width="80" align="center">
          <template slot-scope="scope">{{scope.row.id}}</template>
        </el-table-column>
        <el-table-column label="厂商图标" width="180" align="center">
          <template slot-scope="scope"><img style="width:120px;" :src="scope.row.pic"></template>
        </el-table-column>
        <el-table-column label="代理商名" align="center">
          <template slot-scope="scope">{{scope.row.brandName}}</template>
        </el-table-column>
        <el-table-column label="联系地址" width="120" align="center">
          <template slot-scope="scope">{{scope.row.payType | formatPayType}}</template>
        </el-table-column>
        <el-table-column label="负责人" width="120" align="center">
          <template slot-scope="scope">{{scope.row.sourceType | formatSourceType}}</template>
        </el-table-column>
        <el-table-column label="电话" width="120" align="center">
          <template slot-scope="scope">{{scope.row.totalAmount}}</template>
        </el-table-column>
        <el-table-column label="添加时间" width="180" align="center">
          <template slot-scope="scope">{{scope.row.createTime | formatCreateTime}}</template>
        </el-table-column>


        <el-table-column label="代理商数" width="120" align="center">
          <template slot-scope="scope">{{scope.row.status | formatStatus}}</template>
        </el-table-column>
        <el-table-column label="操作" width="200" align="center">
          <template slot-scope="scope">
            <el-button
              size="mini"
              @click="handleViewOrder()">查看详情</el-button>
            <el-button
              size="mini"
              @click="handleDeleteOrder(scope.$index, scope.row)"
              v-show="scope.row.status===1||scope.row.status===4||scope.row.status===2||scope.row.status===3||scope.row.status===0">删除用户</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>
    <div class="pagination-container">
      <el-pagination
        background
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        layout="total, sizes,prev, pager, next,jumper"
        :current-page.sync="listQuery.pageNum"
        :page-size="listQuery.pageSize"
        :page-sizes="[5,10,15]"
        :total="total">
      </el-pagination>
    </div>
    <logistics-dialog v-model="logisticsDialogVisible"></logistics-dialog>
  </div>
</template>
<script>
  import {fetchList,closeOrder,deleteOrder} from '@/api/order'
  import {formatDate} from '@/utils/date';
  import {fetchList as fetchBrandList} from '@/api/brand';
  import LogisticsDialog from '@/views/pro/order/components/logisticsDialog';
  const defaultListQuery = {
    pageNum: 1,
    pageSize: 10,
    receiverKeyword: null,
    status: null,
    orderType: null,
    createTime: null,
  };
  export default {
    name: "orderList",
    components:{LogisticsDialog},
    data() {
      return {
        listQuery: Object.assign({}, defaultListQuery),
        listLoading: true,
        list: null,
        total: null,
        operateType: null,
        multipleSelection: [],
        closeOrder:{
          dialogVisible:false,
          content:null,
          orderIds:[]
        },
        brandOptions: [],
        logisticsDialogVisible:false
      }
    },
    created() {
      this.getList();
    },
    filters: {
      formatCreateTime(time) {
        let date = new Date(time);
        return formatDate(date, 'yyyy-MM-dd hh:mm:ss')
      },
    },
    methods: {
      handleResetSearch() {
        this.listQuery = Object.assign({}, defaultListQuery);
      },
      handleSearchList() {
        this.listQuery.pageNum = 1;
        this.getSelect();
      },
      handleSelectionChange(val){
        this.multipleSelection = val;
      },
      handleViewOrder(){
        this.logisticsDialogVisible=true;
      },
      handleCloseOrder(index, row){
        this.closeOrder.dialogVisible=true;
        this.closeOrder.orderIds=[row.id];
      },
      handleViewLogistics(index, row){
        this.logisticsDialogVisible=true;
      },
      handleDeleteOrder(index, row){
        let ids=[];
        ids.push(row.id);
        this.deleteOrder(ids);
      },
      handleSizeChange(val){
        this.listQuery.pageNum = 1;
        this.listQuery.pageSize = val;
        this.getList();
      },
      handleCurrentChange(val){
        this.listQuery.pageNum = val;
        this.getList();
      },
      getSelect() {
        this.listLoading = true;
        fetchList(this.listQuery).then(response => {
          this.listLoading = false;
          this.list = response.data.list;
          this.total = response.data.total;
        });
      },

      getList(){
        this.listLoading = true;
        fetchList(this.listQuery).then(response => {
          this.listLoading = false;
          this.list = response.data.list;
          this.total = response.data.total;
          this.list[0].pic = require('./components/manu/01.jpg');
          this.list[1].pic = require('./components/manu/02.jpg');
          this.list[2].pic = require('./components/manu/03.jpg');
          this.list[3].pic = require('./components/manu/04.jpg');
          this.list[4].pic = require('./components/manu/05.jpg');
          this.list[5].pic = require('./components/manu/06.jpg');
          this.list[6].pic = require('./components/manu/07.jpg');
          this.list[7].pic = require('./components/manu/08.jpg');
          this.list[8].pic = require('./components/manu/09.jpg');
          this.list[9].pic = require('./components/manu/10.jpg');
          this.list[0].brandName="lg";
          this.list[1].brandName="上海家化";
          this.list[2].brandName="索菲娜";
          this.list[3].brandName="古驰";
          this.list[4].brandName="雅漾";
          this.list[5].brandName="雅芳";
          this.list[6].brandName="阿迪达斯";
          this.list[7].brandName="华硕";
          this.list[8].brandName="巴黎世家";
          this.list[9].brandName="卡地亚";
          this.list[0].payType="韩国";
          this.list[1].payType="上海";
          this.list[2].payType="日本";
          this.list[3].payType="意大利";
          this.list[4].payType="法国";
          this.list[5].payType="法国";
          this.list[6].payType="美国";
          this.list[7].payType="台湾";
          this.list[8].payType="法国";
          this.list[9].payType="法国";
          this.list[0].totalAmount=13598423454;
          this.list[1].totalAmount=13584681542;
          this.list[2].totalAmount=14754413813;
          this.list[3].totalAmount=15848263854;
          this.list[4].totalAmount=13758456842;
          this.list[5].totalAmount=13648524522;
          this.list[6].totalAmount=15846822542;
          this.list[7].totalAmount=12355157515;
          this.list[8].totalAmount=15755448726;
          this.list[9].totalAmount=13897157878;

          this.list[0].sourceType=" 具本茂";
          this.list[1].sourceType="张东方";
          this.list[2].sourceType="Michitaka Sawada";
          this.list[3].sourceType="Marco Bizzarri";
          this.list[4].sourceType="Eric Ducournau ";
          this.list[5].sourceType="钟彬娴";
          this.list[6].sourceType="罗思德";
          this.list[7].sourceType="施崇棠";
          this.list[8].sourceType=" Kering SA ";
          this.list[9].sourceType="李汉龙";
        });
      },
      afterdelete() {
        this.listLoading = true;
        fetchList(this.listQuery).then(response => {
          this.listLoading = false;
          this.list = response.data.list;
          this.total = response.data.total;
          //this.list.splice(ids,1);
          this.list[0].pic = require('./components/manu/02.jpg');
          this.list[1].pic = require('./components/manu/03.jpg');
          this.list[2].pic = require('./components/manu/04.jpg');
          this.list[3].pic = require('./components/manu/05.jpg');
          this.list[4].pic = require('./components/manu/06.jpg');
          this.list[5].pic = require('./components/manu/07.jpg');
          this.list[6].pic = require('./components/manu/08.jpg');
          this.list[7].pic = require('./components/manu/09.jpg');
          this.list[8].pic = require('./components/manu/10.jpg');
          this.list[9].pic = require('./components/manu/11.jpg');
          this.list[0].brandName="上海家化";
          this.list[1].brandName="索菲娜";
          this.list[2].brandName="古驰";
          this.list[3].brandName="雅漾";
          this.list[4].brandName="雅芳";
          this.list[5].brandName="阿迪达斯";
          this.list[6].brandName="华硕";
          this.list[7].brandName="巴黎世家";
          this.list[8].brandName="卡地亚";
          this.list[8].brandName="花王";
          this.list[0].payType="上海";
          this.list[1].payType="日本";
          this.list[2].payType="意大利";
          this.list[3].payType="法国";
          this.list[4].payType="法国";
          this.list[5].payType="美国";
          this.list[6].payType="台湾";
          this.list[7].payType="法国";
          this.list[8].payType="法国";
          this.list[9].payType="riben";
          this.list[0].totalAmount=13584681542;
          this.list[1].totalAmount=14754413813;
          this.list[2].totalAmount=15848263854;
          this.list[3].totalAmount=13758456842;
          this.list[4].totalAmount=13648524522;
          this.list[5].totalAmount=15846822542;
          this.list[6].totalAmount=12355157515;
          this.list[7].totalAmount=15755448726;
          this.list[8].totalAmount=13897157878;
          this.list[9].totalAmount=13897157878;
          this.list[0].sourceType="张东方";
          this.list[1].sourceType="Michitaka Sawada";
          this.list[2].sourceType="Marco Bizzarri";
          this.list[3].sourceType="Eric Ducournau ";
          this.list[4].sourceType="钟彬娴";
          this.list[5].sourceType="罗思德";
          this.list[6].sourceType="施崇棠";
          this.list[7].sourceType=" Kering SA ";
          this.list[8].sourceType="李汉龙";
          this.list[9].sourceType="李汉龙";
        });
      },
      deleteOrder(ids){
        this.listLoading = true;
        this.$confirm('是否要进行该删除操作?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.afterdelete();
          this.$message({
            type:"success",
            message:"删除成功"
          });
        });
      },
      covertOrder(order){
        let address=order.receiverProvince+order.receiverCity+order.receiverRegion+order.receiverDetailAddress;
        let listItem={
          orderId:order.id,
          receiverName:order.receiverName,
          receiverPhone:order.receiverPhone,
          receiverPostCode:order.receiverPostCode,
          address:address,
          deliveryCompany:null,
          deliverySn:null
        };
        return listItem;
      }
    }
  }
</script>
<style scoped>
  .input-width {
    width: 203px;
  }
</style>


