<template>
  <el-form>
    <el-row :gutter="10">
      <el-col :span="24">
        <el-form-item>
          <el-input @change="modify" v-model="textarea1" :autosize="{ minRows: 10, maxRows: 10 }" type="textarea"
            placeholder="请输入 JSON 字符串" />
        </el-form-item>
      </el-col>
      <el-col :span="24">
        <el-form-item>
          <div style="border: 1px solid #dcdfe6; padding: 5px; border-radius: 4px; background-color: #f5f7fa;">
            <pre>{{ textarea2 }}</pre>
          </div>
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
