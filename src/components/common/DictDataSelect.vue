<template>
  <div>
    <el-select v-model="param" @clear="clear" @change="handleChangeValue" clearable>
      <el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value"></el-option>
    </el-select>
  </div>
</template>

<script>
import request from "@/utils/request";
import { getDictDatasByDictCode } from "@/utils/baseRequest";

export default {
  name: "commonDictDataSelect",
  data() {
    return {
      options: [],
      param: null
    };
  },
  props: ["dictCode", "dictValue"],

  mounted() {
    //获取各类型数据字典列表
    getDictDatasByDictCode(this.dictCode).then(response => {
      this.options = response.data.map(item => {
        return {
          value: item.dictDataCode,
          label: item.dictDataName
        };
      });
    });
    this.param = this.dictValue;
  },

  methods: {
    clear() {
      this.$emit("clearDictVal");
    },
    handleChangeValue(val) {
      this.$emit("getDictVal", val);
    }
  }
};
</script>

