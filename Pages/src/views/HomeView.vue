<template>
  <div>
    <el-container>
      <el-header>用户管理</el-header>
      <el-main>
        <!-- 新建弹出框-->
        <el-popover placement="right-start" width="500" trigger="click" v-model="visible">
          <el-form :model="list2" ref="ruleForm" label-width="100px" class="demo-ruleForm" >
            <el-form-item label="姓名">
              <el-input v-model="list2.name"></el-input>
            </el-form-item>
             <el-form-item label="年龄">
              <el-input v-model="list2.age"></el-input>
            </el-form-item>
            <el-form-item label="性别">
              <el-select v-model="list2.sex" placeholder="请选择性别">
                <el-option label="男" value="男"></el-option>
                <el-option label="女" value="女"></el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="联系电话">
              <el-input v-model="list2.phone"></el-input>
            </el-form-item>
            <el-form-item label="地址">
              <el-input v-model="list2.address"></el-input>
            </el-form-item>
            <el-form-item>
              <el-button type="primary" @click="add()">新建</el-button>
              <el-button type="primary" @click="visible = false">取消</el-button>
            </el-form-item>
          </el-form>
          <el-button size="mini" slot="reference">新建</el-button>
        </el-popover>

        <el-input v-model="search" size="mini" placeholder="输入关键字搜索" style="width: 200px"/>
        <el-button size="mini" @click="toggleSelection()">取消选择</el-button>
        <!-- 表格-->
        <el-table
          :data="tableData.filter((data) => data.name.includes(search))"
          style="width: 100%"
          ref="table"
          @selection-change="handleSelectionChange"
        >
          <el-table-column type="selection" width="55"></el-table-column>
          <el-table-column label="姓名">
            <template slot-scope="scope">
              <span>{{ scope.row.name }}</span>
            </template>
          </el-table-column>
          <el-table-column label="年龄">
            <template slot-scope="scope">
              <span>{{ scope.row.age }}</span>
            </template>
          </el-table-column>
          <el-table-column label="性别">
            <template slot-scope="scope">
              <span>{{ scope.row.sex }}</span>
            </template>
          </el-table-column>
          <el-table-column label="联系电话">
            <template slot-scope="scope">
              <span>{{ scope.row.phone }}</span>
            </template>
          </el-table-column>
          <el-table-column label="详细地址">
            <template slot-scope="scope">
              <span>{{ scope.row.address }}</span>
            </template>
          </el-table-column>
          <el-table-column align="center" label="操作">
            <template slot-scope="scope">
              <el-popconfirm
                confirm-button-text="好的"
                cancel-button-text="不用了"
                @confirm="handleDelete(scope.$index)"
                icon="el-icon-info"
                icon-color="red"
                title="这是一段内容确定删除吗？"
              >
                <el-button slot="reference" size="mini" type="danger"
                  >删除</el-button
                >
              </el-popconfirm>
              <!-- 编辑弹出框-->
              <el-popover placement="left" width="500" trigger="click">
                <el-form :model="list" ref="ruleForm" label-width="100px" class="demo-ruleForm">
                  <el-form-item label="姓名">
                    <el-input v-model="list.name"></el-input>
                  </el-form-item>
                  <el-form-item label="性别">
                    <el-select v-model="list.sex" placeholder="请选择性别">
                      <el-option label="男" value="男"></el-option>
                      <el-option label="女" value="女"></el-option>
                    </el-select>
                  </el-form-item>
                  <el-form-item label="联系电话">
                    <el-input v-model="list.phone"></el-input>
                  </el-form-item>
                  <el-form-item label="地址">
                    <el-input v-model="list.address"></el-input>
                  </el-form-item>
                  <el-form-item>
                    <el-button type="primary" @click="preservation(scope.$index)">保存</el-button>
                  </el-form-item>
                </el-form>
                <el-button size="mini" slot="reference" @click="handleEdit(scope.$index)">编辑</el-button>
              </el-popover>
            </template>
          </el-table-column>
        </el-table>
        <el-button @click="Selectivitydel()">批量删除</el-button>
      </el-main>
    </el-container>
  </div>
</template>

<script>
export default {
  data() {
    return {
      list: {},
      list2: {
        date: "",
        name: "",
        age: "",
        sex: "",
        phone: "",
        address: "",
      },
      list3: {},
      visible: false,
      tableData: [
        {
          date: "2016-05-02",
          name: "张三",
          age: "11",
          sex: "男",
          phone: "123",
          address: "广东广州",
        },
        {
          date: "2016-05-02",
          name: "李四",
          age: "21",
          sex: "男",
          phone: "123456",
          address: "广东广州 ",
        },
        {
          date: "2016-05-02",
          name: "王五",
          age: "18",
          sex: "男",
          phone: "123",
          address: "广东广州 ",
        },
        {
          date: "2016-05-02",
          name: "刘柳",
          age: "18",
          sex: "女",
          phone: "123",
          address: "广东广州",
        },
        {
          date: "2016-05-02",
          name: "小明",
          age: "13",
          sex: "男",
          phone: "123",
          address: "广东广州",
        },
      ],
      search: "",
    };
  },
  methods: {
    //新建
    add() {
      if(this.list2.name !=''){
        this.tableData.push(this.list2);
        this.visible = false;
      }else{
         this.$alert('<strong>姓名不能为空</strong>', '错误！！！', {
          dangerouslyUseHTMLString: true
        });
      }
      
    },
    //保存
    preservation(index) {
      this.tableData[index] = this.list;
    },
    //取消选择
    toggleSelection() {
      this.$refs.table.clearSelection();
    },
    //编辑
    handleEdit(index) {
      this.list = this.tableData[index];
    },
    //删除
    handleDelete(index) {
      this.tableData.splice(index, 1);
    },

    handleSelectionChange(val) {
      this.list3 = val;
    },
    //批量删除
    Selectivitydel() {
      for (var i = 0; i < this.tableData.length; i++) {
        for (var j = 0; j < this.list3.length; j++) {
          if (this.tableData[i] == this.list3[j]) {
            this.tableData.splice(i, 1);
          }
        }
      }
    },
  },
  created(){
    //加载时获取数据
    this.tableData=JSON.parse(localStorage.getItem("key"))
    //刷新时保存数据
    window.addEventListener("beforeunload" , ()=>{
      localStorage.setItem("key" ,JSON.stringify(this.tableData))
    })
  }
};
</script>
<style scoped>
.el-header {
  background-color: #b3c0d1;
  color: #333;
  text-align: center;
  line-height: 60px;
}
.el-aside {
  background-color: #d3dce6;
  color: #333;
  text-align: center;
  line-height: 200px;
}

.el-main {
  background-color: #e9eef3;
  color: #333;
}
</style>