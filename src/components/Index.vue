

<template>
  <el-row>
    <el-col :span="4" :xs="0"/>
    <el-col :span="16" :xs="24">
      <el-tabs tab-position="top">
        <el-tab-pane label="格式化SQL IN 条件">
          <el-card >
            <TransToSqlIn/>
          </el-card>
        </el-tab-pane>
        <el-tab-pane label="密码生成器">
          <el-card >
            <PasswordGenerator/>
          </el-card>
        </el-tab-pane>
        <el-tab-pane label="Base64">
          <el-card >
            <Base64Converter/>
          </el-card>
        </el-tab-pane>
        <el-tab-pane label="MD5计算器">
          <el-card >
            <MD5Calculator/>
          </el-card>
        </el-tab-pane>
        <el-tab-pane label="时间戳工具">
          <el-card >
            <TimestampConverter/>
          </el-card>
        </el-tab-pane>
      </el-tabs>
    </el-col>
    <el-col :span="4" :xs="0"/>
  </el-row>
</template>

<script setup lang="ts">

import TransToSqlIn from './utils/TransToSqlIn.vue'
import PasswordGenerator from './utils/PasswordGenerator.vue'
import Base64Converter from './utils/Base64Converter.vue'
import MD5Calculator from './utils/MD5Calculator.vue'
import TimestampConverter from './utils/TimestampConverter.vue'

</script>

<style scoped>

</style>
