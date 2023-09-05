<template>
  <el-space
      fill
      wrap
      :fill-ratio="100"
      direction="vertical"
      style="width: 100%"
  >
    <el-row>
      <el-col>
        <el-input
            v-model="text"
            :autosize="{ minRows: 10, maxRows: 20 }"
            type="textarea"
            placeholder="请输入文本"
        />
      </el-col>
    </el-row>
    <el-row>
      <el-col>
        <el-button-group>
          <el-button type="primary" @click="encode">转换为Base64</el-button>
          <el-button type="primary" @click="decode">解码Base64</el-button>
        </el-button-group>
      </el-col>
    </el-row>
    <el-row>
      <el-col>
        <el-input
            v-model="result"
            :autosize="{ minRows: 10, maxRows: 20 }"
            type="textarea"
            placeholder="转换结果"
        />
      </el-col>
    </el-row>
  </el-space>
</template>

<script lang="ts">
import { ref } from 'vue';

export default {
  name: 'Base64Converter',
  setup() {
    const text = ref<string>('');
    const result = ref<string>('');

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

</style>
