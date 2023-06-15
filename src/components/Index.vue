<template>
  <div class="common-layout">
    <el-container>
      <el-header style="height: 10px;color: #42b983">常用小工具</el-header>
      <el-container>

        <el-aside :width="lWidth"></el-aside>
        <el-container>
          <el-main>
            <el-tabs :tab-position="tabPos" class="demo-tabs">
              <el-tab-pane label="格式化SQL IN 条件" style="max-height: 600px; overflow-y: auto;">
                <el-card class="box-card">
                  <p style="text-align: left">可将以换行符分隔的字符串转为 in 查询条件格式</p>
                  <TransToSqlIn/>
                </el-card>
              </el-tab-pane>
              <el-tab-pane label="ClashX节点格式转换" style="max-height: 600px; overflow-y: auto;">
                <el-card class="box-card">
                  <p style="text-align: left">适用于ClashX软件的节点格式转换</p>
                  <SubscribeTrans/>
                </el-card>

              </el-tab-pane>
              <el-tab-pane label="密码生成器" style="max-height: 600px; overflow-y: auto;">
                <el-card class="box-card">
                  <p style="text-align: left">密码生成器</p>
                  <PasswordGenerator/>
                </el-card>
              </el-tab-pane>
              <el-tab-pane label="Base64" style="max-height: 600px; overflow-y: auto;">
                <el-card class="box-card">
                  <p style="text-align: left">Base64</p>
                  <Base64Converter/>
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
        <el-aside :width=rWidth></el-aside>
      </el-container>
    </el-container>
  </div>


</template>

<script lang="ts">
import {defineComponent} from 'vue';
import SubscribeTrans from './Utils/SubscribeTrans.vue'
import TransToSqlIn from './Utils/TransToSqlIn.vue'
import PasswordGenerator from './Utils/PasswordGenerator.vue'
import Base64Converter from './Utils/Base64Converter.vue'


export default defineComponent({
  name: 'Index',
  props: {
    msg: String,
  },
  components: {
    SubscribeTrans, TransToSqlIn,PasswordGenerator,Base64Converter
  },
  mounted() {
    this.IsPhoneView()
  },
  data() {
    return {
      lWidth:'15%',
      rWidth:'25%',
      tabPos: 'left',
    }
  },
  methods:{
    IsPhoneView(){
      let devicesType =navigator.userAgent.match(/(phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i)
      console.log(devicesType)
      if ( devicesType !== null){
        this.tabPos='top'
        this.lWidth='2%'
        this.rWidth='2%'
      }
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
