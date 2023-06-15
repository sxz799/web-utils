<template>
  <el-form>
    <el-row>
      <el-col :span="10">
        <el-input
            v-model="text"
            :autosize="{ minRows: 20, maxRows: 20 }"
            type="textarea"
            placeholder="请输入文本"
        />
      </el-col>

      <el-col :span="4">
        <div class="button">
          <el-button type="primary" @click="encode">转换为Base64</el-button>
        </div>
        <div class="button">
          <el-button type="primary" @click="decode">解码Base64</el-button>
        </div>

      </el-col>

      <el-col :span="10">
        <el-input
            v-model="result"
            :autosize="{ minRows: 20, maxRows: 20 }"
            type="textarea"
            placeholder="转换结果"
        />
      </el-col>
    </el-row>
  </el-form>
</template>

<script>
import { ref } from 'vue';

export default {
  name: 'Base64Converter',
  setup() {
    const text = ref('');
    const result = ref('');

    const encode = () => {
      result.value = window.btoa(text.value);
    };

    const decode = () => {
      result.value = window.atob(text.value);
    };

    return {
      text,
      result,
      encode,
      decode,
    };
  },
};
</script>

<style scoped>
.button {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 10px;
}
</style>
