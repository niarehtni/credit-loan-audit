<!-- 直销人员查询 -->
<template>
  <div class="taskWatting main-div">
    <div @click="dropdownFlag=false" class="wrap">
      <div class="taskWinput search-div">
        <el-row class="row row1" type="flex">
          <el-col :span="6" class="search-item" :offset="0">
            <span class="keywordText">用户名称： </span>
            <el-input v-model.trim="params.userName" placeholder="请输入用户名称"></el-input>
          </el-col>
          <el-col :span="6" class="search-item">
            <span class="keywordText">用户编号：</span>
            <el-input v-model.trim="params.userCode" placeholder="请输入用户编号"></el-input>
          </el-col>
          <el-col :span="6" class="search-item">
            <span class="keywordText">手机号码：</span>
            <el-input v-model.trim="params.tel" placeholder="请输入手机号码"></el-input>
          </el-col>
          <el-col :span="6" class="search-item">
            <!-- <span class="keywordText">所属机构： </span>
            <div @click.stop="getDropDownSelect" class="dropdown" style="display:inline-block;position:relative;">
              <span class="dropdownInput" v-show="subOrg">{{subOrg}}</span>
              <span class="dropdownInput" v-show="!subOrg" style="color:#B5BCCD;">请选择所属机构</span>
              <div class="dropList" v-show="dropdownFlag">
                <el-tree :data="orgDatasEdit" node-key="id" :load="loadNode" lazy :props="defaultProps"
                  :expand-on-click-node='false' @node-click="getItemSelect">
                </el-tree>
              </div>
              <i id="dropdownInput-arrow" class="el-select__caret el-input__icon el-icon-arrow-down" :class="{reverse:isActive,reverse2:!isActive}"></i>
            </div> -->
            <span class="keywordText">所属机构： </span>
            <div @click.stop="getDropDownSelect" class="dropdown" style="display:inline-block;position:relative;">
              <span class="dropdownInput" v-show="subOrg">{{subOrg}}</span>
              <span class="dropdownInput" v-show="!subOrg" style="color:#B5BCCD;">请选择所属机构</span>
              <div class="dropList" v-show="dropdownFlag">
                <el-tree :data="orgDatasEdit" :props="defaultProps" :expand-on-click-node='false' @node-click="getItemSelect"></el-tree>
              </div>
              <i id="dropdownInput-arrow" class="el-select__caret el-input__icon el-icon-arrow-down" :class="{reverse:isActive,reverse2:!isActive}"></i>
            </div>
          </el-col>
        </el-row>
        <el-row class="row row2" type="flex">
          <el-col :span="6" class="search-item">
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
          直销人员列表
        </span>
      </div>
      <div class="listContainer">
        <el-table :data="tableData" style="width: 100%" height="510" highlight-current-row border>
          <el-table-column type="index" label=序号 width="55">
          </el-table-column>
          <el-table-column prop="userName" label="用户名称" width="140">
          </el-table-column>
          <el-table-column prop="userCode" label="用户编号" width="140">
          </el-table-column>
          <el-table-column prop="sexTxt" label="性别" width="80">
          </el-table-column>
          <el-table-column prop="tel" label="手机号码" width="160">
          </el-table-column>
          <el-table-column prop="orgName" label="所属机构" min-width="150">
          </el-table-column>
          <el-table-column prop="subOrgName" label="所属科室" min-width="140">
          </el-table-column>
          <el-table-column prop="registerDate" label="注册日期" width="160">
          </el-table-column>
          <el-table-column prop="logoutDate" label="注销日期" width="160">
          </el-table-column>
          <el-table-column prop="validFlagTxt" label="用户状态" width="80">
          </el-table-column>
        </el-table>
        <div class="page">
          <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :page-sizes="[10, 20,50]"
            :current-page.sync="params.pageNum" :page-size="params.pageSize" layout="total, sizes, prev, pager, next, jumper"
            :total="this.totalRecord">
          </el-pagination>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
  export default {
    data() {
      return {
        tableData: [],
        params: {
          userName: '',
          userCode: '',
          tel: '',
          validFlag: '',
          orgName: '',
          pageNum: 1,
          pageSize: 10
        },
        totalRecord: 0, //总条数
        Routes: this.$router.options.routes,
        subOrg: '',
        orgCode: "",
        orgDatasEdit: [],
        defaultProps: {
          children: "children",
          label: "orgName",
          // isLeaf: 'leaf',
        },
        dropdownFlag: false,
        isActive: false,
        Status: [{
            label: '正常',
            value: '0'
          },
          {
            label: '休息',
            value: '1'
          }, {
            label: '注销',
            value: '2'
          }
        ]
      }
    },
    watch: {
      '$route'(to, from) {
        if (to.path == '/DirectSeller') {
          if (!this.Routes[2].closed) {
            this.Rreset();
            this.Routes[2].closed = true;
          }
        }
      },
    },
    methods: {
      getinstitution() {
        this.post('/credit/getSmOrg', {
          orgCode: this.orgCode,
          isCurrentOrgCode: '1',
          isVerifySysInfo: '01'
        }).then((res) => {
          if (res.statusCode == 200) {
            this.orgDatasEdit = res.data;
          } else {
            this.$message.error(res.msg)
          }
        })
      },
      // 打开下拉菜单里的树形结构
      getDropDownSelect(event) {
        // 查询的所属机构
        if ((!this.dropdownFlag && event.target.className === 'dropdownInput') || (!this.dropdownFlag && event.target.id ===
            'dropdownInput-arrow')) {
          this.dropdownFlag = true;
          this.isActive = true;
        } else {
          this.dropdownFlag = false;
        }
      },
      // 查询的树形结构选取某一级数据，所属机构
      getItemSelect(row, node, arr) {
        this.dropdownFlag = false;
        this.subOrg = row.orgName;
        if (this.subOrg) {
          this.isActive = false;
        };
        this.params.orgName = row.orgName;
      },
      // 点击展开时加载
      // loadNode(node, resolve) {
      //   var data;
      //   if (node.data.state == "closed") {
      //     this.post('/credit/getSmOrg', {
      //       id: node.data.id,
      //       isCurrentOrgCode: '0',
      //     }).then((res) => {
      //       if (res.statusCode == 200) {
      //         data = res.data;
      //         for (var i = 0, len = data.length; i < len; i++) {
      //           if (data[i].state === 'open') {
      //             data[i].leaf = true;
      //           }
      //         }
      //         return resolve(data);
      //       }
      //     })
      //   } else {
      //     return resolve([])
      //   }
      // },
      Rreset() {
        this.params.userName = '';
        this.params.userCode = '';
        this.params.tel = '';
        this.params.validFlag = '';
        this.params.orgName = '';
        this.subOrg = '';
        this.totalRecord = 0;
        this.tableData = []; ////清空已查处列表
      },
      Rsearch() {
        if (this.params.userCode != '' || this.params.userName != '' || this.params.tel != '' || this.params.validFlag !=
          '' || this.params.orgName != '') {
          this.inquire(this.params);
        } else {
          this.$message.error('请输入查询条件')
        }
      },
      handleSizeChange(val) { //每页 N 条
        this.params.pageSize = val;
        this.params.pageNum = 1;
        this.inquire(this.params);
      },
      handleCurrentChange(val) { //查看第 N 页
        this.params.pageNum = val;
        this.inquire(this.params);
      },
      inquire(pam) {
        this.post("/credit/filteredSalePer", pam).then(res => {
          if (res.statusCode == 200) {
            this.tableData = res.data.recordList;
            this.totalRecord = res.data.totalRecord;
          } else {
            this.tableData = [];
            this.totalRecord = 0;
            this.$message.error(res.msg);
          }
        })
      },
    },
    created() {
      this.orgCode = JSON.parse(localStorage.getItem('userInf')).orgCode;
      this.getinstitution();
    }
  }

</script>
<style scoped>
  .wrap {
    height: 100%;
    width: 100%;
  }

  .dropList {
    position: absolute;
    background-color: #fff;
    border: 1px solid #dfe4ed;
    border-radius: 4px;
    box-shadow: 0 2px 12px 0 rgba(0, 0, 0, .1);
    height: 235px;
    width: 100%;
    background: #fff;
    z-index: 111;
    overflow: auto;
    margin-top: 12px;
  }

</style>
