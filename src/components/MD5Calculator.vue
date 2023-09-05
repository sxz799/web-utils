<template>
  <el-form>
    <div class="md5-calculator">
      <el-form-item label="输入文本">
        <el-input
            type="textarea"
            v-model="inputText"
            placeholder="在这里输入文本"
        ></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="calculateMD5">计算MD5</el-button>
      </el-form-item>
      <el-form-item>
        <div class="md5-results">
          <div v-for="result in md5Results" :key="result.id" class="md5-result">
            <label>{{ result.label }}：</label>
            <span>{{ result.value }}</span>
          </div>
        </div>
      </el-form-item>
    </div>
  </el-form>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import { ElMessage } from 'element-plus';
import md5 from 'md5'; // 导入 MD5 库

export default defineComponent({
  name: 'MD5Calculator',
  setup() {
    const inputText = ref('');
    const md5Results = ref([]);

    const calculateMD5 = () => {
      if (!inputText.value) {
        ElMessage.warning('请输入要计算的文本！');
        return;
      }

      const originalHash = md5(inputText.value);
      md5Results.value = [
        {
          id: 1,
          label: '小写16位',
          value: originalHash.substr(8, 16),
        },
        {
          id: 2,
          label: '小写32位',
          value: originalHash,
        },
        {
          id: 3,
          label: '大写16位',
          value: originalHash.substr(8, 16).toUpperCase(),
        },
        {
          id: 4,
          label: '大写32位',
          value: originalHash.toUpperCase(),
        },
      ];
    };

    return {
      inputText,
      md5Results,
      calculateMD5,
    };
  },
});
</script>

<style scoped>
.md5-calculator {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
}

.md5-results {
  margin-top: 10px;
}

.md5-result {
  margin-top: 10px;
}

.md5-result label {
  font-weight: bold;
}

.md5-result span {
  word-break: break-all;
  display: inline-block;
}
</style>
