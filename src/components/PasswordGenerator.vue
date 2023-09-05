<template>
  <el-form>
    <div class="password-generator">
      <el-form-item>
        <el-input
            v-model="password"
            placeholder="生成的密码"
            readonly
        ></el-input>
      </el-form-item>
      <div class="slider-wrapper">
        <el-form-item>
          <el-slider
              v-model="length"
              :min="4"
              :max="30"
              :step="1"
              show-input
              :input-controls="false"
              :show-tooltip="false"
              :format-tooltip="formatTooltip"
          ></el-slider>
        </el-form-item>
      </div>
      <el-form-item>
        <el-checkbox-group v-model="options">
          <el-checkbox label="uppercase">大写字母</el-checkbox>
          <el-checkbox label="lowercase">小写字母</el-checkbox>
          <el-checkbox label="numbers">数字</el-checkbox>
          <el-checkbox label="symbols">特殊符号</el-checkbox>
        </el-checkbox-group>
      </el-form-item>
      <el-form-item label="排除的字符:">
        <el-input v-model="excludedChars"></el-input>
      </el-form-item>

      <el-button type="primary" @click="generatePassword">生成密码</el-button>
      <el-button type="success" @click="copyPassword" :disabled="!password">复制</el-button>
    </div>
  </el-form>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import { ElMessage } from 'element-plus';

export default defineComponent({
  name: 'PasswordGenerator',
  setup() {
    const excludedChars = ref('lI10oO');
    const password = ref('');
    const length = ref(12);
    const options = ref<string[]>(['uppercase', 'lowercase', 'numbers']);

    const generatePassword = () => {
      const characterSets: Record<string, string> = {
        uppercase: 'ABCDEFGHIJKLMNOPQRSTUVWXYZ',
        lowercase: 'abcdefghijklmnopqrstuvwxyz',
        numbers: '0123456789',
        symbols: '!@#$%^&*()_+',
      };

      const validOptions = options.value.filter(option => option !== '');

      if (validOptions.length === 0) {
        ElMessage.warning('请至少选择一种填充内容！');
        return;
      }

      let characters = validOptions.map(option => characterSets[option]).join('');

      const excludedCharacters = excludedChars.value;
      characters = characters.replace(new RegExp(`[${excludedCharacters}]`, 'g'), '');

      let result = '';

      for (let i = 0; i < length.value; i++) {
        const randomIndex = Math.floor(Math.random() * characters.length);
        result += characters[randomIndex];
      }

      password.value = result;
    };

    const copyPassword = () => {
      navigator.clipboard.writeText(password.value).then(() => {
        ElMessage.success('密码已复制到剪贴板！');
      }).catch((error) => {
        console.error('复制密码失败：', error);
        ElMessage.error('复制密码失败！');
      });
    };

    const formatTooltip = () => {
      return `${length.value} 位密码`;
    };

    return {
      excludedChars,
      password,
      length,
      options,
      generatePassword,
      copyPassword,
      formatTooltip,
    };
  },
});
</script>

<style scoped>
.password-generator {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
}

.slider-wrapper {
  margin: 10px;
}
</style>
