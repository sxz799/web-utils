<template>
  <div class="common-layout">
    <el-container>
      <el-container>
        <el-aside width="7%"></el-aside>
        <el-container>
          <el-main>
            <el-tabs tab-position="left"  class="demo-tabs">

              <el-tab-pane label="格式化SQL IN 条件">
                <el-card class="box-card">
                  <p style="text-align: left">可将以换行符分隔的字符串转为 in 查询条件格式</p>
                  <TransToSqlIn/>
                </el-card>
              </el-tab-pane>
              <el-tab-pane label="订阅格式转换">
                <el-card class="box-card">
                  <p style="text-align: left">适用于Clash软件的订阅格式转换</p>
                  <SubscribeTrans/>
                </el-card>

              </el-tab-pane>
              <el-tab-pane label="...">
                <el-card class="box-card">
                 ...
                </el-card>
              </el-tab-pane>
              <el-tab-pane label="...">
                <el-card class="box-card">
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

export default defineComponent({
  name: 'Index',
  props: {
    msg: String,
  },
  components: {
    SubscribeTrans,TransToSqlIn
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
