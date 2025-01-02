<template>
  <el-form>
    <el-row :gutter="20">
      <el-col :span="10">
        <el-form-item>
          <el-input v-model="textarea1" :autosize="{ minRows: 6, maxRows: 6 }" type="textarea"
            placeholder="Please input" />
        </el-form-item>
        <el-form-item style="display: flex; flex-direction: column; justify-content: center; align-items: center;">
          <el-button type="primary" @click="modify">转换 =></el-button>
        </el-form-item>
      </el-col>

      <el-col :span="14">
        <el-form-item>
          <pre>{{ textarea2 }}</pre>
        </el-form-item>
      </el-col>
    </el-row>

  </el-form>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import { ElMessage } from 'element-plus';

export default defineComponent({
  name: "JsonFormatter",
  setup() {
    const textarea1 = ref('');
    const textarea2 = ref('');

    const isValidJson = (str: string) => {
      try {
        JSON.parse(str);
        return true;
      } catch (e) {
        return false;
      }
    };

    const formatJson = (str: string) => {
      return JSON.stringify(JSON.parse(str), null, 2);
    };

    const modify = () => {
      if (textarea1.value === "") {
        ElMessage.info("请填写要转换的内容！");
        return;
      }

      if (!isValidJson(textarea1.value)) {
        ElMessage.error("输入的字符串不是有效的 JSON 格式！");
        return;
      }

      textarea2.value = formatJson(textarea1.value);
      ElMessage.success("转换完成！");
    };

    return {
      textarea1,
      textarea2,
      modify,
    };
  },
});
</script>

<style scoped></style>
