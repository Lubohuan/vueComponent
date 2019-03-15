<template>

  <el-table :data="rows" border @selection-change="onSelectionChange"  style="width: 100%">
  <!-- <el-table ref="currentTable" :data="rows" border @selection-change="onSelectionChange"  @current-change="onCurrentChange" style="width: 100%"> -->
    <!-- 复选框  列 -->
    <!-- <el-table-column fixed type="selection" width="55"></el-table-column> -->
    <el-table-column type="index" label="序号" width="45" align="center"></el-table-column>
    <!-- 操作 列-->
    <el-table-column v-if="hasConfig" width="150" label="操作" align="center" fixed="right">
      <template slot-scope="scope">
        <el-button size="mini" type="primary" @click="editClick(scope.row)">编辑</el-button>
        <el-button size="mini" type="info" @click="cancleEdit(scope.row)">取消</el-button>
      </template>
    </el-table-column>
    <!-- tbody -->
    <el-table-column align="left" v-for="(column,index) in columns" :key="index" :width="column.width" :prop="column.prop">
      <!-- thead -->
      <template slot="header" slot-scope="scope">
        <!-- 是否排序 -->
        <p v-if="column.sortAbled"  @click.stop="changeOrder(column)"  class="div-inBlock" style="cursor:pointer;display:inline-block;height:100%;">
          <span>{{column.title}}</span>
          <span class="caret-wrapper">
            <i class="sort-caret ascending" :style="{'border-bottom-color':column.sortOrder == 'asc' ? '#409eff' : '#c0c4cc'}"></i>
            <i class="sort-caret descending" :style="{'border-top-color':column.sortOrder == 'desc' ? '#409eff' : '#c0c4cc'}"></i>
         </span>
        </p>
        <p v-else class="div-inBlock">
            <label style="cursor:pointer">{{column.title}}</label>
        </p >
        <!-- 是否过滤 -->
        <p v-if="column.filterAbled" class="div-inBlock">
          <el-popover :ref="'filterPanel'+index" v-model="column.filterPanelVisible" placement="bottom" width="230" trigger="click">
            <i class="el-icon-arrow-down" slot="reference" @click.stop="showPanel(column)"></i>
            <el-input  size="small" :placeholder="column.title" clearable v-model="column.filterValue" class="input-with-select" @keyup.enter.native="onFilter('filterPanel'+index,column)">
              <el-button slot="append" icon="el-icon-search" @click.stop="onFilter('filterPanel'+index,column)"></el-button>
            </el-input>
          </el-popover>
        </p>
      </template> 
     
      <!-- tbody -->
      <template slot-scope="scope">
          <!--当前行编辑时 -->
          <label v-if="scope.row.editAbled">
            <div v-if="column.cellRender=='text'">
              <el-form :model="scope.row" :rules="tableRules" :ref="'table_form'+column.prop" label-width="0">
                  <el-form-item label="" :prop="column.prop">
                      {{scope.row[column.prop]}}
                  </el-form-item>
              </el-form>
            </div>
            <div v-if="column.cellRender=='input'">
              <el-form :model="scope.row" :rules="tableRules" :ref="'table_form'+column.prop" label-width="0">
                  <el-form-item label="" :prop="column.prop">
                      <el-input v-model="scope.row[column.prop]"></el-input>
                  </el-form-item>
              </el-form>
            </div>
          </label>
          <!--当前行非编辑时 -->
          <label v-else>
              {{scope.row[column.prop]}}
          </label>
      </template>
    </el-table-column>
  </el-table>
</template>
<script>
export default {
  props:['tableRules'],
  data() {
    return {
      columns:null,
      rows:null,
      hasConfig:false   
    };
  },
  methods: {
    init(columns,hasConfig){//初始化表头
    if(hasConfig){
      this.hasConfig = true
    }else{
      this.hasConfig = false
    }
      columns.forEach(column=>{
        column.sortOrder = "none";
        column.filterValue = "";
      })
      this.columns = columns;
    },
    load(rows){//rows为表格数据
      this.rows = rows
    },
    getColumns(){
      return this.columns;
    },
    showPanel(column){//展示过滤面板
      column.filterPanelVisible = true;
    },
    changeOrder(column){//排序方法
      var destOrder = column.sortOrder =="none"?"asc":
                      column.sortOrder =="asc"?"desc":
                      column.sortOrder =="desc"?"none":"none";
      this.columns.forEach(column=>column.sortOrder="none");
      column.sortOrder = destOrder;
      //this.$emit("on-query");
    },
    onFilter(ref,column){//过滤方法
      column.filterValue = column.filterValue;
      column.filterPanelVisible = false; //或者 this.$refs[ref].forEach(x=>x.doClose())**用来关闭过滤面板**
     
      //this.$emit("on-query");
    },
    editClick(row) {
      this.rows.forEach(row=>row.editAbled = false);
      row.editAbled = true;
    },
    cancleEdit(row) {
      this.rows.forEach(row=>row.editAbled = false)
    },
    onSelectionChange(selectedRows) {//多选
      this.$emit("selection-change",selectedRows)
    },
    onCurrentChange(current) {//当前行选中
      //this.$emit("current-change",current)
    },
    setCurrentRow(row){
      this.$refs.currentTable.setCurrentRow(row)
    }
  }
};
</script>