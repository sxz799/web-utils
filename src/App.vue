

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
        <el-tab-pane label="JSON格式化">
          <el-card >
            <JsonFormatter/>
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

import TransToSqlIn from './components/TransToSqlIn.vue'
import PasswordGenerator from './components/PasswordGenerator.vue'
import Base64Converter from './components/Base64Converter.vue'
import MD5Calculator from './components/MD5Calculator.vue'
import TimestampConverter from './components/TimestampConverter.vue'
import JsonFormatter from './components/JsonFormatter.vue'

</script>

<style scoped>

</style>
