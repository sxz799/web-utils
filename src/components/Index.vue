<template>
  <div class="common-layout">
    <el-container>
      <el-header style="height: 10px;color: #42b983">常用小工具</el-header>
      <el-container>

        <el-aside width="7%"></el-aside>
        <el-container>
          <el-main>
            <el-tabs :tab-position="tabPos" class="demo-tabs">
              <el-tab-pane label="格式化SQL IN 条件" style="max-height: 600px; overflow-y: auto;">
                <el-card class="box-card">
                  <p style="text-align: left">可将以换行符分隔的字符串转为 in 查询条件格式</p>
                  <TransToSqlIn/>
                </el-card>
              </el-tab-pane>
              <el-tab-pane label="订阅格式转换" style="max-height: 600px; overflow-y: auto;">
                <el-card class="box-card">
                  <p style="text-align: left">适用于ClashX软件的订阅格式转换</p>
                  <SubscribeTrans/>
                </el-card>

              </el-tab-pane>
              <el-tab-pane label="密码生成器" style="max-height: 600px; overflow-y: auto;">
                <el-card class="box-card">
                  <p style="text-align: left">密码生成器</p>
                  <PasswordGenerator/>
                </el-card>
              </el-tab-pane>
              <el-tab-pane label="...">
                <el-card class="box-card" style="max-height: 600px; overflow-y: auto;">
                  ...
                </el-card>
              </el-tab-pane>
            </el-tabs>


          </el-main>
        </el-container>
        <el-aside width="10%"></el-aside>
      </el-container>
    </el-container>
  </div>


</template>

<script lang="ts">
import {defineComponent} from 'vue';
import SubscribeTrans from './Utils/SubscribeTrans.vue'
import TransToSqlIn from './Utils/TransToSqlIn.vue'
import PasswordGenerator from './Utils/PasswordGenerator.vue'

export default defineComponent({
  name: 'Index',
  props: {
    msg: String,
  },
  components: {
    SubscribeTrans, TransToSqlIn,PasswordGenerator
  },
  data() {
    return {
      tabPos: navigator.userAgent.match(/(phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i) === null ? "left" : "top"
    }
  }
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
