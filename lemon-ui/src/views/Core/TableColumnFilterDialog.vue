<template>
  <!--表格显示列界面-->
  <el-dialog :close-on-click-modal="false" :visible.sync="dialogVisible" title="表格显示列" width="40%">
    <el-table :data="columns" :size="size" @selection-change="selectionChange" align="left" header-align="left"
              height="330px"
              ref="fitlerTable" style="width: 100%" tooltip-effect="dark">
      <el-table-column type="selection" width="40">
      </el-table-column>
      <el-table-column label="属性">
        <template slot-scope="scope">{{ scope.row.prop }}</template>
      </el-table-column>
      <el-table-column label="列名">
        <template slot-scope="scope">
          <el-input :size="size" v-model="scope.row.label"></el-input>
        </template>
      </el-table-column>
      <el-table-column label="最小宽度">
        <template slot-scope="scope">
          <el-input :size="size" v-model="scope.row.minWidth"></el-input>
        </template>
      </el-table-column>
    </el-table>
    <div class="dialog-footer" slot="footer">
      <el-button :size="size" @click.native="dialogVisible = false">{{ $t('action.cancel') }}</el-button>
      <el-button :size="size" @click.native="handleFilterColumns" type="primary">{{ $t('action.comfirm') }}</el-button>
    </div>
  </el-dialog>
</template>

<script>
export default {
  name: 'TableColumnFilterDialog',
  components: {},
  props: {
    columns: {
      type: Array,
      default: []
    },
    size: {
      type: String,
      default: 'mini'
    }
  },
  data() {
    return {
      selections: [],  // 列表选中列
      dialogVisible: false
    }
  },
  methods: {
    // 选择切换
    selectionChange: function (selections) {
      this.selections = selections

    },
    // 设置可见性
    setDialogVisible: function (visible) {
      this.dialogVisible = visible
    },
    // 处理表格列过滤显示
    handleFilterColumns: function () {
      let filterColumns = []
      for (let i = 0; i < this.columns.length; i++) {
        let column = this.columns[i]
        if (this.hasColumn(column)) {
          filterColumns.push(column)
        }
      }
      this.$emit('handleFilterColumns', {filterColumns: JSON.parse(JSON.stringify(filterColumns))})
    },
    hasColumn: function (column) {
      for (let i = 0; i < this.selections.length; i++) {
        let col = this.selections[i]
        if (column.prop == col.prop) {
          return true
        }
      }
      return false
    }
  },
  mounted() {
  }
}
</script>

<style scoped>

</style>