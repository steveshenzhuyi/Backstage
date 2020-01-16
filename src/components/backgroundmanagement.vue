<template>
  <div class="css3">
    <el-tabs>
      <el-tab-pane label="人员信息维护">

        <component is="perinfomaintain"></component>

      </el-tab-pane>
      <el-tab-pane label="会议信息维护">
        <component is="conferinfomaintain"></component>
      </el-tab-pane>
    </el-tabs>

  </div>
</template>

<script>
import axios from 'axios';
import perinfomaintain from './backgroundmanage/perinfomaintain'
import conferinfomaintain from './backgroundmanage/conferinfomaintain'
export default {
  components: {

    'perinfomaintain': perinfomaintain,
    'conferinfomaintain': conferinfomaintain
  },
  data() {
    return {

    }

  },
  methods: {

  }
}
</script>

<style>
.css3 {
  height: 894px;
}
.css3 .el-tabs__item:hover {
  color: #fdac60;
}
.css3 .el-tabs__item.is-active {
  color: #fdc577;
}
.css3 .el-tabs__item {
  color: #f2f6fc;
}
.css3 .el-tabs--border-card > .el-tabs__header .el-tabs__item {
  font-size: 16px;
}
</style>

