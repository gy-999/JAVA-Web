<template>
  <div>
    <el-container style="height: 700px; border: 1px solid #eee">
      <el-header style="font-size:40px;background-color: rgb(238, 241,246)">tlias 智能学习辅助系统</el-header>
      <el-container>
        <el-row class="tac">
          <el-col :span="30">
            <h5>自定义颜色</h5>
            <el-menu
                default-active="2"
                class="el-menu-vertical-demo"
                @open="handleOpen"
                @close="handleClose"
                background-color="#545c64"
                text-color="#fff"
                active-text-color="#ffd04b">
              <el-submenu index="1">
                <template slot="title">
                  <i class="el-icon-location"></i>
                  <span>系统信息</span>
                </template>
                <el-submenu index="1-1">
                  <template slot="title">管理</template>
                  <el-menu-item index="1-1">员工管理</el-menu-item>
                  <el-menu-item index="1-2">部门管理</el-menu-item>
                </el-submenu>
                <el-submenu index="1-2">
                  <template slot="title">选项2</template>
                  <el-menu-item index="1-2">选项2</el-menu-item>
                </el-submenu>
                <el-submenu index="1-3">
                  <template slot="title">选项3</template>
                  <el-menu-item index="1-3">选项3</el-menu-item>
                </el-submenu>
                <el-submenu index="1-4">
                  <template slot="title">选项4</template>
                  <el-menu-item index="1-4">选项4</el-menu-item>
                </el-submenu>
              </el-submenu>

              <el-menu-item index="2">
                <i class="el-icon-menu"></i>
                <span slot="title">导航二</span>
                <el-menu-item index="2-1">选项1</el-menu-item>
              </el-menu-item>
              <el-menu-item index="3">
                <i class="el-icon-document"></i>
                <span slot="title">导航三</span>
                <el-menu-item index="3-1">选项1</el-menu-item>
              </el-menu-item>
              <el-menu-item index="4">
                <i class="el-icon-setting"></i>
                <span slot="title">导航四</span>
                <el-menu-item index="4-1">选项1</el-menu-item>
              </el-menu-item>
            </el-menu>
          </el-col>
        </el-row>
        <el-main>

          <el-form :inline="true" :model="searchForm" class="demo-form-inline">
            <el-form-item label="姓名">
              <el-input v-model="searchForm.user" placeholder="姓名"></el-input>
            </el-form-item>
            <el-form-item label="">
              <el-select v-model="searchForm.region" placeholder="性别">
                <el-option label="男" value="male"></el-option>
                <el-option label="女" value="female"></el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="入职日期">
              <el-date-picker
                  v-model="value2"
                  type="daterange"
                  align="right"
                  unlink-panels
                  range-separator="至"
                  start-placeholder="开始日期"
                  end-placeholder="结束日期"
                  :picker-options="pickerOptions">
              </el-date-picker>
            </el-form-item>
            <el-form-item>
              <el-button type="primary" @click="onSubmit">查询</el-button>
            </el-form-item>

          </el-form>

          <el-table :data="tableData">
            <el-table-column prop="name" label="姓名" width="180"></el-table-column>
            <el-table-column prop="image" label="图像" width="180"></el-table-column>
            <el-table-column prop="gender" label="性别" width="140"></el-table-column>
            <el-table-column prop="job" label="职位" width="140"></el-table-column>
            <el-table-column prop="entrydate" label="入职日期" width="180"></el-table-column>
            <el-table-column prop="updatetime" label="最后操作时间" width="230"></el-table-column>
            <el-table-column label="操作" >
              <el-button type="primary" size="mini">编辑</el-button>
              <el-button type="danger" size="mini">删除</el-button>
            </el-table-column>
          </el-table>
          <el-pagination
              @size-change="handleSizeChange"
              @current-change="handleCurrentChange"
              background
              layout="sizes,prev, pager, next,jumper,total"
              :total="1000">
          </el-pagination>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>


<script>
  import axios from 'axios';
  export default {
    name: 'EmpView',
    mounted(){
      axios.get("")
          .then(resp=>{
            this.tableData=resp.data.data; //响应数据赋值给数据模型
          });
    },
    data() {
      return {
        tableData: [{
          name: '张三',
          image: 'https://picsum.photos/200/300',
          gender: '男',
          job: '程序员',
          entrydate: '2020-01-01',
          updatetime: '2020-01-01 12:00:00'
        }],
        searchForm: {
          user: '',
          region: ''
        },
        methods: {
          handleSizeChange(val) {
            console.log(`每页 ${val} 条`);
          },
          handleCurrentChange(val) {
            console.log(`当前页: ${val}`);
          }
        },
      }
    }
  }
</script>
