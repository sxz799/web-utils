<template>
  <el-form>

    <el-form-item>
      <el-checkbox v-model="splitSpace" label="空格作为分隔符" size="large"/>
      <el-checkbox v-model="includeParentheses" label="结果加上括号" size="large"/>
      <el-checkbox v-model="wrapResult" label="结果换行显示" size="large"/>
      <el-checkbox v-model="numType" label="不带单引号" size="large"/>
    </el-form-item>
    <el-form-item>
      <el-input
          v-model="textarea1"
          :autosize="{ minRows: 6, maxRows: 6 }"
          type="textarea"
          placeholder="Please input"
      />
    </el-form-item>

    <el-form-item style="display: flex;flex-direction: column;justify-content: center;align-items: center;">

      <el-button type="primary" @click="modify">转换</el-button>
      <el-button type="success" @click="copy">复制</el-button>

    </el-form-item>

    <el-form-item>
      <el-input
          v-model="textarea2"
          :autosize="{ minRows: 6, maxRows: 6 }"
          type="textarea"
          placeholder="Please input"
      />
    </el-form-item>


  </el-form>


</template>

<script>
import useClipboard from 'vue-clipboard3';
import {ElMessage} from 'element-plus';


const {toClipboard} = useClipboard();
export default {
  name: "TransToSqlIn",
  data() {
    return {
      splitSpace: false,
      wrapResult: true,
      includeParentheses: false,
      numType: false,
      textarea1: '',
      textarea2: ''
    };
  },
  methods: {
    copy() {
      toClipboard(this.textarea2)
      ElMessage.success("复制成功！");
    },
    modify() {
      if (this.textarea1 === "") {
        ElMessage.info("请填写要转换的内容！");
        return
      }
      this.textarea2 = this.modifyInput(this.textarea1)
      ElMessage.success("转换完成！");
    },
    modifyInput(input) {

      let newString = input
      let searchString;
      let replaceString;

      //如果空格也作为分隔符就将一个或多个连续的空格转为逗号
      if (this.splitSpace) {
        searchString = /\s+/g;
        replaceString = ",";
        newString = newString.replace(searchString, replaceString);
        //如果最后一个字符是逗号 就去掉它
      }
      //将一个或多个连续的换行符转为逗号
      searchString = /\n+/gi;
      replaceString = ",";
      newString = newString.replace(searchString, replaceString);
      if (newString.endsWith(',')) {
        newString = newString.slice(0, -1);
      }
      //整理成正确的格式
      searchString = /,/gi;
      replaceString = "','";
      newString = newString.replace(searchString, replaceString);
      newString = "'" + newString + "'"

      //处理为换行
      if (this.wrapResult) {
        searchString = /','/gi;
        replaceString = "',\n'";
        newString = newString.replace(searchString, replaceString);
      }

      if (this.numType){
        searchString = /'/gi;
        replaceString = "";
        newString = newString.replace(searchString, replaceString);
      }

      //处理勾选了添加括号的情况
      if (this.includeParentheses) {
        if (this.wrapResult) {
          newString = "(\n" + newString + "\n)"
        } else {
          newString = "(" + newString + ")"
        }
      }


      return newString
    }
  }
}
</script>

<style scoped>

</style>
