<template>
  <div>
    <!--  质检复议流程- 合规质检结论轨迹 -->
    <el-table :data="tableData" border style="width: 100%">
      <el-table-column prop="taskNameTxt" label="任务节点" width="120">
      </el-table-column>
      <el-table-column prop="activationTime" label="生成任务时间" width="190">
      </el-table-column>
      <el-table-column prop="taskStatusTxt" label="任务状态" width="100">
      </el-table-column>
      <el-table-column prop="operatorCode" label="处理人" width="100">
      </el-table-column>
      <el-table-column prop="completeTime" label="任务完成时间" width="190">
      </el-table-column>
      <el-table-column prop="approvalOpinionTxt" label="处理结论" width="80">
      </el-table-column>
      <el-table-column prop="opinionExplain" label="备注" min-width="100">
      </el-table-column>
    </el-table>
  </div>
</template>
<script>
  export default {
    data() {
      return {
        processInstanceId: '',
        tableData: [],
      }
    },
    props: ['propQT'],
    mounted() {
      this.processInstanceId = this.propQT.tastwaitingPass.processInstanceId;
      this.get('/creauditInfo/getProcessTraceList?processInstanceId=' + this.processInstanceId + '&' + Math.random())
        .then(res => {
          if (res.statusCode == '200') {
            this.tableData = res.data;
          } else {
            this.$message(res.msg);
          }
        })
    }
  }

</script>
