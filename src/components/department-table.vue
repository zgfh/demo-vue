<template>
  <div class="container">
    <div class="toolbar">
      <el-form @submit.native.prevent inline>
        <el-form-item>
          <el-input v-model="search" placeholder="请输入名称进行搜索" clearable />
        </el-form-item>
      </el-form>
      <el-button @click="handleCreate" type="success">新增</el-button>
    </div>
    <el-table :data="tableData" border>
      <el-table-column type="index" />
      <el-table-column label="部门名称" prop="name"></el-table-column>
      <el-table-column label="部门经理" prop="manager"></el-table-column>
      <el-table-column align="right">
        <template slot-scope="scope">
          <el-button size="mini" @click="handleEdit(scope.row)">编辑</el-button>
          <el-button
            size="mini"
            type="danger"
            @click="handleDelete(scope.$index, scope.row)"
            >删除</el-button
          >
        </template>
      </el-table-column>
    </el-table>

    <department-dialog
      :visible.sync="departmentFormDialog.visible"
      :data="departmentFormDialog.data"
      :lastId="departmentList.length ? departmentList[departmentList.length - 1].id : 0"
      @create="onCreate"
      @edit="onEdit"
    ></department-dialog>
  </div>
</template>

<style lang="scss" scoped>
.container {
  padding: 20px;
  .toolbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
    .el-form-item {
      margin-bottom: 0;
    }
  }
}
</style>

<script>
import DepartmentDialog from "./department-dialog.vue";
export default {
  components: {
    DepartmentDialog,
  },
  computed: {
    tableData() {
      return this.departmentList.filter(
        (data) =>
          !this.search ||
          data.name.toLowerCase().includes(this.search.toLowerCase())
      );
    },
  },
  data() {
    return {
      departmentFormDialog: {
        visible: false,
        data: {},
      },
      departmentList: [
        {
          id: 1,
          name: "开发部",
          manager: "许昌",
        },
        {
          id: 2,
          name: "宣传部",
          manager: "玛丽",
        },
        {
          id: 3,
          name: "销售部",
          manager: "韩愈",
        },
        {
          id: 4,
          name: "运营部",
          manager: "陈宇",
        },
      ],
      search: "",
    };
  },
  methods: {
    onCreate(data) {
      this.departmentList = [...this.departmentList, data];
    },
    onEdit(data) {
      const index = this.departmentList.findIndex((item) => item.id === data.id);
      this.departmentList.splice(index, 1, data);
    },
    handleCreate() {
      this.departmentFormDialog = {
        visible: true,
        data: {},
      };
    },
    handleEdit(row) {
      this.departmentFormDialog = {
        visible: true,
        data: Object.assign({}, row),
      };
    },
    async handleDelete(index) {
      try {
        await this.$confirm("您是否确认删除？", "提示", { type: "warning" });
        this.departmentList.splice(index, 1);
      } catch (err) {
        console.log(err);
      }
    },
  },
};
</script>
