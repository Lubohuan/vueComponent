<template>
<div>
  <subTable ref="subTableInParent" :tableRules="tableRules" @current-change="onCurrentChange" @selection-change="onSelectionChange" @on-query="refreshTable"></subTable>  
</div>
</template>
<script>
import subTable from '@/components/commonSub/subTable.vue';
export default {
  components:{
    subTable
  },
  data() {
    return {
      tableRules:null,
      tableData: [
        {
          unitName: "2016-05-02",
          proJectTotal: "王小虎",
          hehe:"hehe",
        },
        {
          unitName: "2016-05-02",
          proJectTotal: "王小虎",
          hehe:"1231"
        }
      ],
      columns: [
        {
          title: "单位名称",
          prop: "unitName", //后台返回的key
          width: "",
          required: false, //是否出现“*”
          cellRender: "text",//返回的值为text/input（输入框）/select（下拉框）/date（日期插件）
          sortAbled: true, //能否排序
          sortOrder: "none", //按照哪种方式排序
          filterAbled: false, //能否过滤
          filterValue: "",
          filterPanelVisible:false
        },
        {
          title: "项目总数",
          prop: "proJectTotal", //后台返回的key
          width: "",
          required: false, //是否出现“*”
          cellRender: "text",
          sortAbled: true, //能否排序
          sortOrder: "none", //按照哪种方式排序
          filterAbled: false, //能否过滤
          filterValue: "",
          filterPanelVisible:false
        },
        {
          title: "hehe",
          prop: "hehe", //后台返回的key
          width: "",
          required: false, //是否出现“*”
          cellRender: "text",
          sortAbled: true, //能否排序
          sortOrder: "none", //按照哪种方式排序
          filterAbled: false, //能否过滤
          filterValue: "",
          filterPanelVisible:false
        }
      ]
      
    };
  },
  methods: {
    refreshTable(){
      var params = {};
      //获取排序参数
      var orderColumns = this.$refs.subTableInParent.getColumns().filter(column=>column.sortOrder!='none');
      if(orderColumns[0]){
        params.sortProp = orderColumns[0].prop;
        params.sortOrder = orderColumns[0].sortOrder;        
      }
      //获取过滤的参数
      var filterColumns = this.$refs.subTableInParent.getColumns().filter(column=>column.filterValue);
      if(filterColumns){
        filterColumns.forEach((column,index) => {
            params['filter'+column.prop] = column.prop;
            params['filterValue'+column.prop] = orderColumns[0].filterValue
        });
      }
      //请求后台，获取表格数据，
      //请求后台，获取表格数据之后的回调
      //this.tableData = res.body.data;
      this.$refs.subTableInParent.load(this.tableData)
    },
    onSelectionChange(selectedRows){

    },
    onCurrentChange(currentRow){

    }
  },
  created(){
  },
  mounted(){
    this.$refs.subTableInParent.init(this.columns);
    this.refreshTable();
  }
};
</script>