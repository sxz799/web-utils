<template>
  <el-form>
    <el-form-item>
      <el-input
          v-model="textarea1"
          :autosize="{ minRows: 6, maxRows: 6 }"
          type="textarea"
          placeholder="请将要转换的内容粘贴在此处"
      />
    </el-form-item>

    <el-form-item style="display: flex; flex-direction: column; justify-content: center; align-items: center;">
      <el-row>
        <el-col :span="12" >
          <el-input
              v-model="keyword"
              placeholder="内容过滤"
          ></el-input>
        </el-col>
        <el-col :span="12" >
          <el-button type="primary" @click="modify">转换</el-button>
          <el-button type="success" @click="copy">复制</el-button>
        </el-col>
      </el-row>


    </el-form-item>


    <el-form-item>
      <el-input
          v-model="textarea2"
          :autosize="{ minRows: 6, maxRows: 6 }"
          type="textarea"
          placeholder="转换后的结果显示在这里..."
      />
    </el-form-item>

    <el-form-item>
      <el-input
          v-model="textarea3"
          :autosize="{ minRows: 6, maxRows: 6 }"
          type="textarea"
          placeholder="转换后的结果显示在这里..."
      />
    </el-form-item>


  </el-form>


</template>

<script>
import useClipboard from 'vue-clipboard3';
import {ElMessage} from 'element-plus';


const {toClipboard} = useClipboard();
export default {
  name: "SubscribeTrans",
  data() {
    return {
      textarea1: '',
      textarea2: '',
      textarea3: '',
      keyword: ''
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
      this.textarea3 = this.modifyName(this.textarea1)
      ElMessage.success("转换完成！");
    },
    filterLines(text, char) {
      const lines = text.split('\n');
      const filteredLines = lines.filter(function (line) {
        return line.includes(char);
      });
      return filteredLines.join('\n');
    },
    modifyInput(input) {
      let searchString = '';
      let replaceString = '';
      let newString = input

      if (this.keyword.length > 0) {
        newString = this.filterLines(newString, this.keyword)
      }

      searchString = /\"/gi;
      replaceString = "";
      newString = newString.replace(searchString, replaceString);

      searchString = / /gi;
      replaceString = "";
      newString = newString.replace(searchString, replaceString);

      searchString = /:/gi;
      replaceString = " : ";
      newString = newString.replace(searchString, replaceString);


      searchString = /{/gi;
      replaceString = "\n    ";
      newString = newString.replace(searchString, replaceString);

      searchString = /,/gi;
      replaceString = "\n    ";
      newString = newString.replace(searchString, replaceString);

      searchString = /}/gi;
      replaceString = "";
      newString = newString.replace(searchString, replaceString);

      searchString = /-\n/gi;
      replaceString = "  -\n";
      newString = newString.replace(searchString, replaceString);

      return newString
    },
    modifyName(input) {

      let searchString = '';
      let replaceString = '';
      let newString = input


      if (this.keyword.length > 0) {
        newString = this.filterLines(newString, this.keyword)
      }


      searchString = /\"/gi;
      replaceString = "";
      newString = newString.replace(searchString, replaceString);

      searchString = / /gi;
      replaceString = "";
      newString = newString.replace(searchString, replaceString);


      searchString = /:/gi;
      replaceString = "";
      newString = newString.toString().replace(searchString, replaceString);
      const startString = '{name';
      const endString = ',';
      const regex = new RegExp(`${startString}(.*?)${endString}`, 'g');

      const matches = [...newString.matchAll(regex)];
      const contents = matches.map(match => match[1]);
      searchString = /,/gi;
      replaceString = "\n      - ";
      newString = contents.toString().replace(searchString, replaceString);


      newString = "      - " + newString
      return newString
    }
  }
}
</script>

<style scoped>

</style>