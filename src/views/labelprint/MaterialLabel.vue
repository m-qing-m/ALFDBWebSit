<template>
  <div>
    <el-card>
      <el-form  size="small"  :model="queryParam"  >
        <el-row>
          <el-col :span="6" hidden="hidden">
            <el-form-item label="">
              <el-input  placeholder="采购订单号" clearable></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="">
              <el-input v-model="queryParam.Materialno" placeholder="物料编码" clearable></el-input>
            </el-form-item>
          </el-col>

          <el-col :span="8">
            <el-form-item>
              <el-button icon="el-icon-search" type="primary"  @click="SelectList()">查询</el-button>
              <el-button icon="el-icon-refresh-right" type="primary">重置</el-button>
            </el-form-item>
          </el-col>

        </el-row>
      </el-form>
    </el-card>

    <el-card body-style="padding:2px;">
      <el-table border :data="DataList" :header-cell-style="{ padding: '2px', background: '#f6f6f6' }" v-loading="loading"
        :cell-style="{ padding: '2px' }" style="width: 100%;">
        <template v-for="item in columns">
          <el-table-column :key="item.prop" :prop="item.prop" :label="item.label" :width="item.width"
            v-if="item.colvisible" show-overflow-tooltip>
          </el-table-column>
       
        </template>
        <el-table-column width="100" fixed="right" label="操作">
          <template slot-scope="scope">
            <el-button @click="PrintLabel(scope.row)" type="text" size="small">打印</el-button>
          </template>
        </el-table-column>
      </el-table>
    </el-card>   
      <purchaseLabel ref="purchaseDialog"></purchaseLabel>
  </div>
</template>

<script>
import {
    getMaterilOrderList
  } from "@/api/api";

import  purchaseLabel from  "./PurchaseLabelDialog"
  export default {
    name: "purchase",
     components:{
      getMaterilOrderList:getMaterilOrderList,
           purchaseLabel:purchaseLabel
    },
    data() {
      return {
        dialogtitle:"物料标签打印",
           queryParam: {
          Materialno:""
        },
        DataList:[],
        // idshow: false,
        // apiUrl: {
        //   query: "/Purchase/GetT_PurchaseOrderListADFAsync",
        //   save: "",
        //   update: ""
        // },
            columns: [{
            label: "Id",
            prop: "Id",
            colvisible: false
            },{
            label: "物料订单号",
            prop: "Erpvoucherno",
            colvisible: false
            },{
            label: "物料编码",
            prop: "Materialno",
            colvisible: true
            }
            ,{
            label: "物料名称",
            prop: "Materialdesc",
            colvisible: true
            }
            ,{
            label: "规格型号",
            prop: "Spec",
            colvisible: true
            }
            ,{
            label: "订单数量",
            prop: "Voucherqty",
            colvisible: false
            } ,{
            label: "已收数量",
            prop: "Receiveqty",
            colvisible: false
            },{
            label: "未收数量",
            prop: "Remainqty",
            colvisible: false
            },{
            label:"包装量",
              prop:"Packqty",
              colvisible:true
            },{
               label:"批次",
              prop:"Batchno",
              colvisible:false
            },
            {label:"打印数量",
              prop:"Printqty",
              colvisible:false
            }, {
              label:"单据类型",
              prop:"Vouchertype",
              colvisible:false
            }
            ]
      }
    },methods:{
   SelectList() {
     debugger;
         var json= JSON.stringify(this.queryParam);
            getMaterilOrderList(json).then(res => {
            if (res.Result === 1) {
              this.DataList = res.Data;
            }else{
              alert(res.ResultValue);
            }
        });
      },
      PrintLabel:function (record) {
        //打印
        debugger;
        if(record.PackQty==undefined){
          record.PackQty=0;
        }
       if(record.Remainqty==undefined){
            record.Remainqty
        }
        record.Printqty=0;
        record.Vouchertype=1;
        this.$refs.purchaseDialog.print(record,this.dialogtitle);
      }
    }
  }

</script>

<style lang="scss" scoped>
>>>.el-dialog{
  margin:5vh auto 50px !important;
}
>>>.el-dialog__title{
  font-size: 14px;
  color: #fff;
}
>>>.el-dialog__header{
  background-color:#3b9bf5e6;
}
.el-form-item{
  margin: 10px;
}
</style>
