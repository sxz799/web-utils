<template>
  <div class="password-generator">
    <el-input
        v-model="password"
        placeholder="生成的密码"
        readonly
    ></el-input>
    <div class="slider-wrapper">
      <el-slider
          v-model="length"
          :min="6"
          :max="20"
          :step="1"
          show-input
          :input-controls="false"
          :show-tooltip="false"
          :format-tooltip="formatTooltip"
      ></el-slider>
    </div>
    <el-checkbox-group v-model="options">
      <el-checkbox label="uppercase">大写字母</el-checkbox>
      <el-checkbox label="lowercase">小写字母</el-checkbox>
      <el-checkbox label="numbers">数字</el-checkbox>
      <el-checkbox label="symbols">特殊符号</el-checkbox>
    </el-checkbox-group>
    <el-button type="primary" @click="generatePassword">生成密码</el-button>
    <el-button type="success" @click="copyPassword" :disabled="!password">复制</el-button>
  </div>
</template>

<script>
import { ref } from 'vue';
import { ElMessage } from 'element-plus';

export default {
  name: 'PasswordGenerator',
  setup() {
    const password = ref('');
    const length = ref(12);
    const options = ref(['uppercase', 'lowercase', 'numbers']);

    const generatePassword = () => {
      const characterSets = {
        uppercase: 'ABCDEFGHIJKLMNOPQRSTUVWXYZ',
        lowercase: 'abcdefghijklmnopqrstuvwxyz',
        numbers: '0123456789',
        symbols: '!@#$%^&*()_+',
      };

      let validOptions = options.value.filter(option => option !== '');
      let characters = '';

      if (validOptions.length === 0) {
        ElMessage.warning('请至少选择一种填充内容！');
        return;
      }

      validOptions.forEach(option => {
        characters += characterSets[option];
      });

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
      password,
      length,
      options,
      generatePassword,
      copyPassword,
      formatTooltip,
    };
  },
};
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