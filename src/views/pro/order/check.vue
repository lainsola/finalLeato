<template> 
  <div class="app-container">
    <el-card class="filter-container" shadow="never">
      <div>
        <i class="el-icon-search"></i>
        <span>筛选搜索</span>
        <el-button
          style="float: right"
          @click="handleSearchList()"
          type="primary"
          size="small">
          查询结果
        </el-button>
        <el-button
          style="float: right;margin-right: 15px"
          @click="handleResetSearch()"
          size="small">
          重置
        </el-button>
      </div>
      <div style="margin-top: 15px">
        <el-form :inline="true" :model="listQuery" size="small">
          <el-form-item label="输入搜索：">
            <el-input v-model="listQuery.keyword" placeholder="厂家名称"></el-input>
          </el-form-item>
          <el-form-item label="联系电话：">
            <el-input v-model="listQuery.productSn" placeholder="联系电话："></el-input>
          </el-form-item>
          <el-form-item label="审核状态：">
            <el-select v-model="listQuery.verifyStatus" placeholder="全部" clearable>
              <el-option
                v-for="item in verifyStatusOptions"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
            </el-select>
          </el-form-item>
        </el-form>
      </div>
    </el-card>
    <el-card class="operate-container" shadow="never">
      <i class="el-icon-tickets"></i>
      <span>数据列表</span>
    </el-card>
    <div class="table-container">
      <el-table ref="productTable"
                :data="list"
                style="width: 100%"
                @selection-change="handleSelectionChange"
                v-loading="listLoading"
                border>
        <el-table-column type="selection" align="center"></el-table-column>
        <el-table-column label="编号" align="center">
          <template slot-scope="scope">{{scope.row.id}}</template>
        </el-table-column>
        <el-table-column label="厂商商标" align="center">
          <template slot-scope="scope"><img style="height: 80px" :src="scope.row.pic"></template>
        </el-table-column>
        <el-table-column label="厂家名称" align="center">
          <template slot-scope="scope">
            <p>{{scope.row.brandName2}}</p>
          </template>
        </el-table-column>
        <el-table-column label="联系电话" align="center">
          <template slot-scope="scope">
            <p>{{scope.row.productSn}}</p>
          </template>
        </el-table-column>
        <el-table-column label="所在地" align="center">
          <template slot-scope="scope">
            <p>{{scope.row.mudidi}}</p>
          </template>
        </el-table-column>
        <el-table-column label="注册时间" align="center">
          <template slot-scope="scope">2018-09-15 12:24:27</template>
        </el-table-column>
        <el-table-column label="审核状态" align="center">
          <template slot-scope="scope">
            <p>{{scope.row.verifyStatus | verifyStatusFilter}}</p>
          </template>
        </el-table-column>
        <el-table-column label="操作" align="center">
          <template slot-scope="scope">
            <p>
              <el-button
                size="mini"
                @click="handleViewLogistics()">查看
              </el-button>
              <el-button
              size="mini"
              @click="handleCloseOrder(scope.$index, scope.row)"
              v-show="scope.row.status===0">通过</el-button>
              <el-button
                size="mini" v-show="scope.row.status===1"
                @click="handleDelete(scope.$index, scope.row)">审核
              </el-button>
            </p>
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
        :page-size="listQuery.pageSize"
        :page-sizes="[5,10,15]"
        :current-page.sync="listQuery.pageNum"
        :total="total">
      </el-pagination>
    </div>
    <logistics-dialog v-model="logisticsDialogVisible"></logistics-dialog>
  </div>
</template>
<script>
  import {
    fetchList,
    updateDeleteStatus,
    updateNewStatus,
  } from '@/api/product'
  import LogisticsDialog from '@/views/pro/order/components/detail';
  import {fetchList as fetchSkuStockList,update as updateSkuStockList} from '@/api/skuStock'
  import {fetchList as fetchProductAttrList} from '@/api/productAttr'
  import {fetchList as fetchBrandList} from '@/api/brand'
  import {fetchListWithChildren} from '@/api/productCate'
  const defaultListQuery = {
    keyword: null,
    pageNum: 1,
    pageSize: 5,
    verifyStatus: null,
    productSn: null,
    productCategoryId: null,
  };
  export default {
    components:{LogisticsDialog},
    name: "productList",
    data() {
      return {
        closeOrder:{
          dialogVisible:false,
          content:null,
          orderIds:[]
        },
        brandOptions: [],
        logisticsDialogVisible:false,
        editSkuInfo:{
          dialogVisible:false,
          productId:null,
          productSn:'',
          productAttributeCategoryId:null,
          stockList:[],
          productAttr:[],
          keyword:null,
          mudidi:[],
          status: 1

        },
        listQuery: Object.assign({}, defaultListQuery),
        list: null,
        total: null,
        listLoading: true,

        multipleSelection: [],
        productCateOptions: [],
        verifyStatusOptions: [{
          value: 1,
          label: '审核通过'
        }, {
          value: 0,
          label: '未审核'
        }]
      }
    },
    created() {
      this.getList();
      this.getBrandList();
      this.getProductCateList();
    },
    watch: {
    },
    filters: {
      verifyStatusFilter(value) {
        if (value === 1) {
          return '审核通过';
        } else {
          return '未审核';
        }
      }
    },
    methods: {
      getProductSkuSp(row, index) {
        if (index === 0) {
          return row.sp1;
        } else if (index === 1) {
          return row.sp2;
        } else {
          return row.sp3;
        }
      },
      getList() {
        this.listLoading = true;
        fetchList(this.listQuery).then(response => {
          this.listLoading = false;
          this.list = response.data.list;
          this.list[0].pic = require('./components/01.png');
          this.list[1].pic = require('./components/02.jpg');
          this.list[2].pic = require('./components/03.jpg');
          this.list[3].pic = require('./components/04.jpg');
          this.list[4].pic = require('./components/05.jpg');
          this.list[0].status=1;
          this.list[1].status=1;
          this.list[2].status=1;
          this.list[3].status=1;
          this.list[4].status=1;
          this.total = response.data.total;
        });

      },
      getBrandList() {
        fetchBrandList({pageNum: 1, pageSize: 100}).then(response => {
          this.brandOptions = [];
          let brandList = response.data.list;
          //for (let i = 0; i < brandList.length; i++) {
            //this.brandOptions.push({label: brandList[i].name, value: brandList[i].id});
         // }
          this.list[0].brandName2="华为";
          this.list[1].brandName2="雅诗兰黛";
          this.list[2].brandName2="巴黎欧莱雅";
          this.list[3].brandName2="香奈儿";
          this.list[4].brandName2="宝格丽";



          this.list[0].mudidi="中国";
          this.list[1].mudidi="美国";
          this.list[2].mudidi="法国";
          this.list[3].mudidi="法国";
          this.list[4].mudidi="意大利";

          this.list[0].productSn=400886888;
          this.list[1].productSn=4008207000;
          this.list[2].productSn=	95085123;
          this.list[3].productSn=4001200500;
          this.list[4].productSn=4000006699;
        });
      },
      getProductCateList() {
        fetchListWithChildren().then(response => {
          let list = response.data;
          this.productCateOptions = [];
          for (let i = 0; i < list.length; i++) {
            let children = [];
            if (list[i].children != null && list[i].children.length > 0) {
              for (let j = 0; j < list[i].children.length; j++) {
                children.push({label: list[i].children[j].name, value: list[i].children[j].id});
              }
            }
            this.productCateOptions.push({label: list[i].name, value: list[i].id, children: children});
          }
        });
      },
      handleSearchEditSku(){
        fetchSkuStockList(this.editSkuInfo.productId,{keyword:this.editSkuInfo.keyword}).then(response=>{
          this.editSkuInfo.stockList=response.data;
        });
      },
      
      handleSearchList() {
        this.listQuery.pageNum = 1;
        this.getList();
      },
      handleSizeChange(val) {
        this.listQuery.pageNum = 1;
        this.listQuery.pageSize = val;
        this.getList();
      },
      handleCurrentChange(val) {
        this.listQuery.pageNum = val;
        this.getList();
      },
      handleSelectionChange(val) {
        this.multipleSelection = val;
      },
      handleResetSearch() {
        this.listQuery = Object.assign({}, defaultListQuery);
      },
      handleDelete(index, row){

        this.$confirm('是否审核通过?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.list[index].verifyStatus = 1;
          this.list[index].status = 0;
        });
      },

      handleViewLogistics(){
        this.logisticsDialogVisible=true;
      },
    }
  }
</script>
<style></style>


