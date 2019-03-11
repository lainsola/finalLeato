<template>
  <div class="app-container">

        <div class="total-layout">
              <el-row :gutter="20">
                        
                    <el-col :span="6">

                      <div class="total-frame">
                        <ul>
                          <li class="total-title">累计用户量</li>
                          <li class="total-time">2018-01-01<span>~</span>2019-03-11|上线至今</li>
                          <li class="total-heji">合计</li>
                          <li><span class="total-num-size">18.43</span>万人</li>
                          <li>&nbsp</li>
                          
                        </ul>
                      </div>                       
                    </el-col>

                    <el-col :span="6">

                      <div class="total-frame">
                        <ul>
                          <li class="total-title">昨日|新增用户数</li>
                          <li class="total-time">2019-03-10</li>
                          <li class="total-heji">3-11（日）</li>
                          <li><span class="total-num-size">627</span>人</li>
                          <li>
                            <span class="color-success" style="font-size: 14px">+0.97%</span>
                            <span style="color: #C0C4CC;font-size: 14px">同比昨日</span>
                          </li>
                        </ul>
                      </div>                       
                    </el-col>

                    <el-col :span="6">

                      <div class="total-frame">
                        <ul>
                          <li class="total-title">昨日|访问人数</li>
                          <li class="total-time">2019-03-11<span>~</span>2019-03-11|昨日</li>
                          <li class="total-heji">3.11（日）</li>
                          <li><span class="total-num-size">4,415</span>人</li>
                          <li>
                            <span class="color-success" style="font-size: 14px">+4.23%</span>
                            <span style="color: #C0C4CC;font-size: 14px">同比昨日</span>
                          </li>
                        </ul>
                      </div>                 
                    </el-col>

                    <el-col :span="6">

                      <div class="total-frame">
                        <ul>
                          <li class="total-title"><a href="#">昨日|访问次数</a></li>
                          <li class="total-time">2019-03-11<span>~</span>2019-03-11|昨日</li>
                          <li class="total-heji">3.11（日）</li>
                          <li><span class="total-num-size">7178</span>次</li>
                          <li>
                            <span class="color-success" style="font-size: 14px">+3.62%</span>
                            <span style="color: #C0C4CC;font-size: 14px">同比昨日</span>
                          </li>
                        </ul>
                      </div>                      
                    </el-col>

               
              </el-row>
        </div>



        <div class="click-web-layout">
          <div class="layout-title"><a href="#">每日访问趋势</a>
          <div class="total-time">2018-01-01<span>~</span>2019-03-10|上线至今</div></div>
          
          <div class="click-web--content">

            <el-row>
            <el-col :span="4">
              <div style="padding: 20px">
                <div>
                  <div style="color: #909399;font-size: 14px">3-11（日）</div>
                  <div style="color: #606266;font-size: 24px;padding: 10px 0">44516 <span style="color: #909399;font-size: 14px">人/次</span></div>
                  <div>
                    <span class="color-success" style="font-size: 14px">+10%</span>
                    <span style="color: #C0C4CC;font-size: 14px">同比昨日</span>
                  </div>
                </div>
                <div style="margin-top: 20px;">
                  <div style="color: #909399;font-size: 14px">订单总数</div>
                  <div style="color: #606266;font-size: 24px;padding: 10px 0">17691</div>
                  <div>
                    <span class="color-danger" style="font-size: 14px">-10%</span>
                    <span style="color: #C0C4CC;font-size: 14px">同比昨日</span>
                  </div>
                </div>
                <div style="margin-top: 20px;">
                  <div style="color: #909399;font-size: 14px">销售总额</div>
                  <div style="color: #606266;font-size: 24px;padding: 10px 0">444万</div>
                  <div>
                    <span class="color-success" style="font-size: 14px">+10%</span>
                    <span style="color: #C0C4CC;font-size: 14px">同比昨日</span>
                  </div>
                </div>
                <div style="margin-top: 20px;">
                  <div style="color: #909399;font-size: 14px">本周销售总额</div>
                  <div style="color: #606266;font-size: 24px;padding: 10px 0">3509万</div>
                  <div>
                    <span class="color-danger" style="font-size: 14px">-10%</span>
                    <span style="color: #C0C4CC;font-size: 14px">同比上周</span>
                  </div>
                </div>
              </div>
            </el-col>
            <el-col :span="20">
              <div style="padding: 10px;border-left:1px solid #DCDFE6">
                <el-date-picker
                  style="float: right;z-index: 1"
                  size="small"
                  v-model="orderCountDate"
                  type="daterange"
                  align="right"
                  unlink-panels
                  range-separator="至"
                  start-placeholder="开始日期"
                  end-placeholder="结束日期"
                  @change="handleDateChange"
                  :picker-options="pickerOptions">
                </el-date-picker>
                <div>
                  
                    <ve-line :data="lineChartData" :settings="lineChartSettings" :mark-point="lineChartMarkPoint"></ve-line>
        
                </div>

                
              </div>
            </el-col>
          </el-row>
           
          </div>
        </div>

        
       <div class="total-layout">
            <el-row :gutter="20">
                      
                <el-col :span="6">
                  <div class="total-frame">
                    <ul>
                      <li class="total-title">昨日|人均使用次数</li>
                      <li class="total-time">2019-03-10</li>
                      
                      <li><span class="total-num-size">1.17</span>次</li>
                      <li>
                            <span class="color-danger" style="font-size: 14px">-0.85%</span>
                            <span style="color: #C0C4CC;font-size: 14px">同比昨日</span>
                      </li>
                    </ul>
                  </div>
                </el-col>

                <el-col :span="6">
                  <div class="total-frame">
                    <ul>
                      <li class="total-title">昨日|人均使用时长</li>
                      <li class="total-time">2019-03-10</li>    
                      <li><span class="total-num-size">5,943</span>秒</li>
                      <li>
                            <span class="color-success" style="font-size: 14px">+1.16%</span>
                            <span style="color: #C0C4CC;font-size: 14px">同比昨日</span>
                      </li>
                    </ul>
                  </div>
                </el-col>

                <el-col :span="6">
                  <div class="total-frame">
                    <ul>
                      <li class="total-title">昨日|单次使用平均时长</li>
                      <li class="total-time">2019-03-10</li>
                      
                      <li><span class="total-num-size">3,046</span>秒</li>
                      <li>
                            <span class="color-success" style="font-size: 14px">+1.34%</span>
                            <span style="color: #C0C4CC;font-size: 14px">同比昨日</span>
                      </li>
                    </ul>
                  </div>                 
                </el-col>

                
                <el-col :span="6">
                  <div class="total-frame">
                    <ul>
                      <li class="total-title"><a href="#">昨日|人均使用深度</a></li>
                      <li class="total-time">2019-03-10</li>
                     
                      <li><span class="total-num-size">17.53</span></li>
                      <li>
                            <span class="color-success" style="font-size: 14px">+1.12%</span>
                            <span style="color: #C0C4CC;font-size: 14px">同比昨日</span>
                      </li>
                    </ul>
                  </div>                 
                </el-col>
             
            </el-row>
        </div>


        <div class="statistics-layout">
              
              <el-row :gutter="20">
                
                <el-col :span="12" class="echarts-item ">
               
                  <div class="total-frame echarts-title">
                    <ul>
                      <li class="total-title">昨日|新用户访问占比</li>
                      <li class="total-time">2019-03-01<span>~</span>2019-03-10|上线至今</li>
                      <li>
                            <span class="color-danger" style="font-size: 14px">-10%</span>
                            <span style="color: #C0C4CC;font-size: 14px">同比昨日</span>
                          </li>
                    </ul>        
                  </div>

                  <div class="total-frame echarts-table">
                    <ve-ring :data="ringChartData" :settings="ringChartSettings"></ve-ring>
                  </div>
                </el-col>

                <el-col :span="12" class="echarts-item ">
               
                  <div class="total-frame echarts-title">
                    <ul>
                      <li class="total-title">访问终端分布国家</li>
                      <li class="total-time">2018-01-01<span>~</span>2019-03-10|昨日</li>
                    </ul>        
                  </div>

                  <div class="total-frame echarts-table">
                    <ve-pie :data="pieChartData" :settings="pieChartSettings"></ve-pie>
                  </div>
                </el-col>

                
                
              </el-row>
        </div>

   </div>
</template>

<script>
  import {str2Date} from '@/utils/date';
  
  




  export default {
    name: 'home',
    data() {

      return {

        lineChartData: {
        columns: ['日期', '成本', '利润', '占比', '其他'],
        rows: [{
          '访问数量': 44516, '日期': '03月1日', '订单数量': 2523, '占比': 0.12, '其他': 100
        }, {
          '访问数量': 12523, '日期': '03月2日', '订单数量': 1823, '占比': 0.345, '其他': 100
        }, {
          '访问数量': 21273, '日期': '03月3日', '订单数量': 2623, '占比': 0.7, '其他': 100
        }, {
          '访问数量': 41263, '日期': '03月4日', '订单数量': 3226, '占比': 0.31, '其他': 100
        }, {
          '访问数量': 31213, '日期': '03月5日', '订单数量': 2383, '占比': 0.12, '其他': 100
        }, {
          '访问数量': 71123, '日期': '03月6日', '订单数量': 4521, '占比': 0.65, '其他': 100
        }, {
          '访问数量': 44516, '日期': '03月7日', '订单数量': 3503, '占比': 0.65, '其他': 100
        }]
      },
      lineChartSettings: {
        metrics: ['访问数量', '订单数量'],
        dimension: ['日期']
      },
      lineChartMarkPoint: {
        data: [{
          name: '最大值',
          type: 'max'
        }]
      },
        ringChartSettings: {
          dimension: '终端类型',
          metrics: '次数'
        },

        ringChartData: {
          columns: [ '终端类型', '次数'],
          rows: [{
            '终端类型': 'web页面浏览人均次数', '次数': 4305
          }, {
             '终端类型': '微信小程序浏览人均次数', '次数': 10236
          }]
        },

         pieChartData: {
        columns: [ '访问人数', '利润'],
        rows: [{
           '访问人数': 123, '国家': '中国'
        }, {
           '访问人数': 1223, '国家': '加拿大'
        }, {
          '访问人数': 2123, '国家': '新西兰'
        }, {
          '访问人数': 4123, '国家': '日本'
        }, {
          '访问人数': 3123, '国家': '菲律宾'
        }, {
          '访问人数': 7123, '国家': '韩国'
        },{
          '访问人数': 71, '国家': '其他'
        }]
      },
      pieChartSettings: {
        dimension: '国家',
        metrics: '访问人数'
      },

        


       
    }
  }
  }
</script>

<style scoped>
  .echarts-item{
  padding-bottom:15px; 
  }
  .app-container {
    margin-top: 20px;
    margin-left: 120px;
    margin-right: 120px;

  }

  .address-layout {
  }

  .total-layout {
    margin-bottom:20px;
  }


  
     .total-frame ul{
  
      list-style:none;
      color: #777C7C;
      font-size: 14px;
      color: #8492A6;
      padding:0;
      margin:0;
     
     
    }
    .total-frame{

    background-color: #F7F9FA;
    display: block; 
    
    border-radius: 3px;
    padding: 14px 20px;

    }
    

    ul .total-title{
      color: #5F6E82;
     
    }

     .total-time{
          font-size: 12px;
          
          color: #8492A6;
          line-height:150%;
     
    }
      ul .total-heji{
        font-size: 13px;
         padding-top:10px;
     
    }
     ul .total-num-size{
          font-size: 40px;
          color: #475669;

    }


 
  .un-handle-layout {
    margin-top: 20px;
    border: 1px solid #DCDFE6;
  }

  .layout-title {
    color: #606266;
    padding: 15px 20px;
    background: #F2F6FC;
    
  }

  .un-handle-content {
    padding: 20px 40px;
  }

  .un-handle-item {
    border-bottom: 1px solid #EBEEF5;
    padding: 10px;
  }

  .overview-layout {
    margin-top: 20px;
  }

  .overview-item-value {
    font-size: 24px;
    text-align: center;
  }

  .overview-item-title {
    margin-top: 10px;
    text-align: center;
  }

  .out-border {
    border: 1px solid #DCDFE6;
  }

  .statistics-layout {
    margin-top: 20px;
    //border: 1px solid #DCDFE6;
  }
</style>
