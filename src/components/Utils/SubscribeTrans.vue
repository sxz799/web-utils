<template>
  <el-form>
    <el-form-item>
      <el-input
          v-model="textarea1"
          :autosize="{ minRows: 6, maxRows: 6 }"
          type="textarea"
          @focus="handleFocus"
          placeholder="请将要转换的内容粘贴在此处"
      />
    </el-form-item>

    <el-form-item>
      <el-row :gutter="10">
        <el-col :span="7">
          <el-radio-group v-model="formatType">
            <el-radio label="Json" size="large">Json</el-radio>
            <el-radio label="Yaml" size="large">Yaml</el-radio>
          </el-radio-group>
        </el-col>

        <el-col :span="7">
          <el-input
              v-model="keyword"
              placeholder="内容过滤"
          ></el-input>
        </el-col>

        <el-col :span="7">
          <el-input
              v-model="delword"
              placeholder="关键字删除"
          ></el-input>
        </el-col>

        <el-col :span="3">
          <el-button type="primary" @click="modify">转换</el-button>
        </el-col>


      </el-row>

    </el-form-item>


    <el-form-item>
      <el-input
          v-model="textarea2"
          :autosize="{ minRows: 6, maxRows: 6 }"
          type="textarea"
          placeholder="转换后的结果显示在这里..."
          @focus="copy1"
      />
    </el-form-item>

    <el-form-item>
      <el-input
          v-model="textarea3"
          :autosize="{ minRows: 6, maxRows: 6 }"
          type="textarea"
          placeholder="转换后的结果显示在这里..."
          @focus="copy2"
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
      formatType: 'Json',
      textarea1: '',
      textarea2: '',
      textarea3: '',
      keyword: '中国,香港,台湾',
      delword: '欢迎订阅yui科技'
    };
  },
  methods: {
    handleFocus() {
      navigator.clipboard.readText().then(text => {
        this.textarea1 = text;
      }).catch(error => {
        ElMessage.error("剪切板内容读取失败！");
      });
    },
    copy1() {
      toClipboard(this.textarea2)
      ElMessage.success("复制成功！");
    },
    copy2() {
      toClipboard(this.textarea3)
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
      const chars = char.split(',');
      const filteredLines = lines.filter(function (line) {
        return chars.some(function (char) {
          return line.includes(char);
        });
      });
      return filteredLines.join('\n');
    },

    deleteWord(text, word) {
      let regex = new RegExp(word, 'g');
      return text.replace(regex, "");
    },
    transFormatYaml(text) {
      let searchString = '';
      let replaceString = '';
      let newString = text

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
    transFormatJson(text) {
      let searchString = '';
      let replaceString = '';
      let newString = text

      searchString = /\"/gi;
      replaceString = "";
      newString = newString.replace(searchString, replaceString);

      searchString = / /gi;
      replaceString = "";
      newString = newString.replace(searchString, replaceString);

      searchString = /:/gi;
      replaceString = " : ";
      newString = newString.replace(searchString, replaceString);

      searchString = /-{/gi;
      replaceString = "    - {";
      newString = newString.replace(searchString, replaceString);
      return newString

    },
    modifyInput(input) {
      let newString = input
      if (this.keyword.length > 0) {
        newString = this.filterLines(newString, this.keyword)
      }
      if (this.delword.length > 0) {
        newString = this.deleteWord(newString, this.delword)
      }

      switch (this.formatType) {
        case "Json":
          newString = this.transFormatJson(newString)
          break
        case "Yaml":
          newString = this.transFormatYaml(newString)
          break
      }

      return newString
    },
    modifyName(input) {

      let searchString = '';
      let replaceString = '';
      let newString = input

      if (this.keyword.length > 0) {
        newString = this.filterLines(newString, this.keyword)
      }
      if (this.delword.length > 0) {
        newString = this.deleteWord(newString, this.delword)
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