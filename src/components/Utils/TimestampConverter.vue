<template>
  <el-form>
    <div class="timestamp-converter">
      <el-form-item label="时间戳">
        <el-input v-model="timestampInput" placeholder="在这里输入时间戳" @blur="handleBlur('timestamp')"></el-input>
      </el-form-item>
      <el-form-item label="时间">
        <el-date-picker
            v-model="selectedDate"
            type="datetime"
            placeholder="选择时间"
            style="width: 100%;"
            @blur="handleBlur('datePicker')"
        ></el-date-picker>
      </el-form-item>
      <el-form-item>
        <div class="conversion-results">
          <div class="conversion-result">
            <label>时间戳：</label>
            <span>{{ timestampResult }}</span>
          </div>
          <div class="conversion-result">
            <label>时间：</label>
            <span>{{ formattedTime }}</span>
          </div>
        </div>
      </el-form-item>
    </div>
  </el-form>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import { ElMessage } from 'element-plus';

export default defineComponent({
  name: 'TimestampConverter',
  setup() {
    const timestampInput = ref('');
    const selectedDate = ref(null); // 使用Element Plus的日期时间选择器
    const timestampResult = ref('');
    const formattedTime = ref('');

    const handleBlur = (component) => {
      if (component === 'timestamp') {
        const timestamp = timestampInput.value.trim();
        if (timestamp) {
          if (/^\d+$/.test(timestamp)) {
            // 输入是时间戳
            const timestampValue = parseInt(timestamp, 10);
            const date = new Date(timestampValue * 1000); // 时间戳是以秒为单位
            timestampResult.value = timestampValue.toString();
            formattedTime.value = formatDateWithDayOfWeek(date);
          } else {
            ElMessage.warning('无法解析输入的时间戳！');
          }
        }
      } else if (component === 'datePicker') {
        const selectedDateTime = selectedDate.value;
        if (selectedDateTime) {
          const date = new Date(selectedDateTime);
          if (!isNaN(date.getTime())) {
            timestampResult.value = Math.floor(date.getTime() / 1000).toString();
            formattedTime.value = formatDateWithDayOfWeek(date);
          } else {
            ElMessage.warning('无法解析选择的时间！');
          }
        }
      }
    };

    // 格式化日期时间，包括年月日时分秒和星期
    const formatDateWithDayOfWeek = (date) => {
      const year = date.getFullYear();
      const month = String(date.getMonth() + 1).padStart(2, '0');
      const day = String(date.getDate()).padStart(2, '0');
      const hours = String(date.getHours()).padStart(2, '0');
      const minutes = String(date.getMinutes()).padStart(2, '0');
      const seconds = String(date.getSeconds()).padStart(2, '0');
      const daysOfWeek = ['星期日', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六'];
      const dayOfWeek = daysOfWeek[date.getDay()];

      return `${year}年${month}月${day}日${hours}:${minutes}:${seconds} ${dayOfWeek}`;
    };

    return {
      timestampInput,
      selectedDate,
      timestampResult,
      formattedTime,
      handleBlur,
    };
  },
});
</script>

<style scoped>
.timestamp-converter {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
}

.conversion-results {
  margin-top: 10px;
}

.conversion-result {
  margin-top: 10px;
}

.conversion-result label {
  font-weight: bold;
}

.conversion-result span {
  word-break: break-all;
  display: inline-block;
}
</style>
