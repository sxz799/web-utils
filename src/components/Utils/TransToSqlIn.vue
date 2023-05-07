<template>
  <el-form>

    <el-form-item>
      <el-checkbox v-model="splitSpace" label="空格作为分隔符" size="large"/>
      <el-checkbox v-model="includeParentheses" label="结果加上括号" size="large"/>
      <el-checkbox v-model="wrapResult" label="结果换行显示" size="large"/>
    </el-form-item>
    <el-form-item>
      <el-input
          v-model="textarea1"
          :autosize="{ minRows: 6, maxRows: 10 }"
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
          :autosize="{ minRows: 6, maxRows: 10 }"
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

      if (this.splitSpace) {
        searchString = /\s+/g;
        replaceString = ",";
        newString = newString.replace(searchString, replaceString);
      }

      searchString = /\n+/gi;
      replaceString = ",";
      newString = newString.replace(searchString, replaceString);

      searchString = /,/gi;
      replaceString = "','";
      newString = newString.replace(searchString, replaceString);

      if (this.wrapResult) {
        searchString = /','/gi;
        replaceString = "',\n'";
        newString = newString.replace(searchString, replaceString);
      }

      newString = "'" + newString + "'"

      if(this.includeParentheses){
        if(this.wrapResult){
          newString = "(\n" + newString + "\n)"
        }else {
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