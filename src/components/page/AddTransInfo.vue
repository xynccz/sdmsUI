
<template>
  <div class="container">
    <div>
      <div style="margin-bottom:5px;">
        <el-button type="danger" icon="el-icon-lx-add" @click="addLine">增加规格明细</el-button>
        <el-tooltip placement="top">
          <div slot="content">
            规格明细的总件数相加需等于PO总件数
            <br>规则明细的总重量相加需等于PO净重
          </div>
          <el-button align="right" icon="el-icon-question">规则说明</el-button>
        </el-tooltip>
      </div>
      <el-table
        :data="tableData"
        style="width: 100%"
        border
        highlight-current-row
        :header-cell-style="{background:'#f4f4f4'}"
        element-loading-text="表格加载中..."
        stripe
      >
        <el-table-column label="序号" type="index" width="60" align="center"></el-table-column>
        <el-table-column prop="specificId" label="规格">
          <template slot-scope="scope">
            <el-select v-model="scope.row.specificId" placeholder="请选择(必填)">
              <el-option
                v-for="item in options"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              ></el-option>
            </el-select>
          </template>
        </el-table-column>
        <el-table-column prop="pieceNum" label="件数">
          <template slot-scope="scope">
            <el-input
              oninput="value=value.replace(/[^\d.]/g,'')"
              v-model="scope.row.pieceNum"
              placeholder="件数(必填)"
              @change="changeInput"
            ></el-input>
          </template>
        </el-table-column>
        <el-table-column prop="weight" label="重量">
          <template slot-scope="scope">
            <el-input
              oninput="value=value.replace(/[^\d.]/g,'')"
              v-model="scope.row.weight"
              placeholder="重量(必填)"
              @change="changeInput"
            ></el-input>
          </template>
        </el-table-column>
        <el-table-column prop="bookbuytime" width="70" label="操作">
          <template slot-scope="scope">
            <el-button
              size="mini"
              type="danger"
              v-if="!scope.row.editing"
              icon="el-icon-delete"
              @click="handleDelete(scope.$index, scope.row)"
            ></el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>
  </div>
</template>
    
<script>
import dictSelect from "@/components/common/DictDataSelect.vue";
import poLine from "@/components/page/PoLine.vue";
import request from "@/utils/request";
import { getDictDatasByDictCode } from "@/utils/baseRequest";
export default {
  name: "addTransInfo",
  data() {
    return {
      tableData: []
    };
  },

  components: {
    dictSelect
  },

  mounted() {},

  methods: {
    //添加行数
    addLine() {
      var newValue = {
        specificId: null,
        pieceNum: null,
        unitPrice: null,
        weight: null,
        remarks: null
      };
      //添加新的行数
      this.tableData.push(newValue);
      this.$emit("getPoLines", this.tableData);
    },
    //删除行数
    handleDelete(index) {
      this.tableData.splice(index, 1);
    },
    changeInput(value) {
      this.$emit("getPoLines", this.tableData);
    }
  }
};
</script>