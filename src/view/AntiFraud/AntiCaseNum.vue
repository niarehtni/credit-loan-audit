<template>
  <div class="taskWatting main-div Height_200">
    <!-- 反欺诈案件编号维护 -->
    <div class="taskWinput search-div">
      <el-row class="row row1" type="flex">
        <el-col :span="6" class="search-item" :offset="0">
          <span class="keywordText">案件编号： </span>
          <el-input v-model="caseNumInput" placeholder="请输入案件编号"></el-input>
        </el-col>
        <el-col :span="6" class="search-item">
        </el-col>
        <el-col :span="6" class="search-item">
        </el-col>
        <el-col :span="6" class="search-btn">
          <el-button class="btn query" type="primary" @click="Rsearch">查询</el-button>
          <el-button class="btn reset" @click="Rreset">重置</el-button>
        </el-col>
      </el-row>
    </div>
    <div class="title titleContainer edit-div">
      <span class="titleText">
        <i class="el-icon title-icon"></i>
        反欺诈申请列表
      </span>
      <span class="iconContainer">
        <span class="icon-item" @click='dialogFormVisible = true'>
          <i class="el-icon faqi"></i>
          <span class="el-icon-text">添加</span>
        </span>
        <span class="icon-item" @click='delList'>
          <i class="el-icon deleteIcon"></i>
          <span class="el-icon-text">删除</span>
        </span>
      </span>
    </div>
    <div class="listContainer">
      <!-- 编辑 -->
      <el-table :data="tableData" style="width: 100%" height="510" highlight-current-row @current-change="handleCurrentChange"
        border>
        <el-table-column type="index" align='center' label=序号 width="50">
        </el-table-column>
        <el-table-column prop="caseNum" label="案件编号" min-width="100">
        </el-table-column>
        <el-table-column prop="creatorCode" label="创建人" align='center' min-width="100">
        </el-table-column>
        <el-table-column prop="creatorDate" label="创建时间" align='center' min-width="100">
        </el-table-column>
        <el-table-column prop="creatorOrgCode" label="创建机构" align='center' min-width="100">
        </el-table-column>
        <el-table-column label="案件描述" min-width="500">
          <template slot-scope="scope">
            <el-popover trigger="hover" placement="top-start">
              <p>{{scope.row.caseDesc}}</p>
              <div slot="reference" class="name-wrapper noBk">
                {{ scope.row.caseDesc }}
              </div>
            </el-popover>
          </template>
        </el-table-column>
      </el-table>
      <div class="page">
        <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :page-sizes="[10, 20,50]" :current-page.sync="currentPage"
          :page-size="pageCount" layout="total, sizes, prev, pager, next, jumper" :total="this.totalRecord">
        </el-pagination>
      </div>
    </div>
    <!-- 添加 -->
    <el-dialog title="添加备案号" :before-close="cal"  :visible.sync="dialogFormVisible">
      <el-form :model="form" :rules="rulesAdd" ref="ruleFormAdd">
        <el-form-item label="案件编号：" prop="caseNum" :label-width="formLabelWidth">
          <el-input v-model="form.caseNum" type="text" placeholder="请输入内容" @compositionend.native="inputCase(form.caseNum)" @keyup.native="inputCase(form.caseNum)"></el-input>
        </el-form-item>
        <el-form-item label="案件描述：" prop="caseDesc" :label-width="formLabelWidth">
          <el-input v-model="form.caseDesc" type='textarea' resize="none" :rows="3" placeholder="请输入内容"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="cal">取 消</el-button>
        <el-button type="primary" @click="sure('ruleFormAdd')">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>
<script>
  export default {
    data() {
      return {
        casNumAlert: false,
        caseNumInput: '',
        dialogFormVisible: false,
        currentRow: null,
        currentPage: 1, //分页选中页
        pageCount: 10, // 每页显示条数
        totalRecord: 0, //总条数
        formLabelWidth: "100px",
        adbtn: '确定',
        loadsitu: false,
        form: {
          caseNum: '',
          caseDesc: '',
        },
        tableData: [],
        rulesAdd: {
          caseNum: [{
            required: true,
            message: '请输入数字',
            trigger: 'blur'
          }],
          caseDesc: [{
              required: true,
              message: '请填写活动形式',
              trigger: 'blur'
            },
            {
              min: 0,
              max: 500,
              message: '输入长度不能超过500',
              trigger: 'blur'
            }
          ]
        }
      }
    },
    methods: {
      handleSizeChange(val) {
        this.pageCount = val;
      },
      handleCurrentChange(val) {
        if (typeof (val) === 'number') {
          this.currentPage = val;
          this.getInf(this.currentPage, this.pageCount, this.caseNumInput);
        } else {
          this.currentRow = val;
        }
      },
      Rreset() {
        this.caseNumInput = '';
      },
      Rsearch() {
        this.getInf(this.currentPage, this.pageCount, this.caseNumInput);
        this.caseNumInput = '';
      },
      delList() {
        if (this.currentRow == '' || this.currentRow == null) {
          this.$message.error('请选中一条数据');
          return
        } else {
          this.post("/caseInfoController/deleteCaseInfo", {
            id: this.currentRow.id
          }).then(res => {
            if (res.statusCode == 200) {
              this.$message({
                message: '删除成功',
                type: 'success'
              });
              // 查询信息
              this.getInf(this.currentPage, this.pageCount, this.caseNumInput);
            } else {
              this.$message.error('删除失败');
            }
          });
        }
      },
      cal() {
        this.dialogFormVisible = false;
        this.form.caseNum = '';
        this.form.caseDesc = '';
      },
      add() {
        this.loadsitu = false;
        this.adbtn = '确定';
        this.dialogFormVisible = true;
      },
      sure(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            this.loadsitu = true;
            this.adbtn = '保存中';
            this.dialogFormVisible = false;
            this.post("/caseInfoController/insert", {
              "param": {
                "caseNum": this.form.caseNum,
                "caseDesc": this.form.caseDesc
              },
            }).then(res => {
              if (res.statusCode == 200) {
                this.$message({
                  message: '提交成功',
                  type: 'success'
                });
                this.getInf(this.currentPage, this.pageCount, this.caseNumInput);
              } else {
                this.$message.error(res.msg);
              }
            });
            this.form.caseNum = '';
            this.form.caseDesc = '';
          } else {
            return false;
          }
        });

      },
      inputCase(val) {
        if (isNaN(val) || val == '') {
          this.form.caseNum = '';
          this.casNumAlert = true;
        } else {
          val = val.replace(/\s+/g, "");
          this.form.caseNum = val;
          this.casNumAlert = false;
        }
      },
      getInf(page, count, val) {
        this.post("/caseInfoController/getCaseInfoList", {
          "pageParam": {
            "pageNum": page,
            "pageSize": count
          },
          "param": {
            "caseNum": val,
          }
        }).then(res => {
          if (res.statusCode == 200) {
            this.totalRecord = res.data.totalRecord;
            this.tableData = res.data.recordList;
          } else {
            this.$message.error(res.msg);
          }
        });
      }
    },
    mounted() {
      this.getInf(this.currentPage, this.pageCount, this.caseNumInput);
    },
  }

</script>
