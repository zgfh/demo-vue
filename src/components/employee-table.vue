<template>
  <div class="container">
    <div class="toolbar">
      <el-form @submit.native.prevent inline>
        <el-form-item>
          <el-input v-model="search" placeholder="请输入姓名进行搜索" clearable />
        </el-form-item>
      </el-form>
      <el-button @click="handleCreate" type="success">新增</el-button>
    </div>
    <el-table :data="tableData" border>
      <el-table-column type="index" />
      <el-table-column label="编号" prop="id"></el-table-column>
      <el-table-column label="姓名" prop="name"></el-table-column>
      <el-table-column label="所在部门" prop="dept"></el-table-column>
      <el-table-column label="职位" prop="position"></el-table-column>
      <el-table-column label="薪资" prop="salary"></el-table-column>
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

    <staff-dialog
      :visible.sync="staffFormDialog.visible"
      :data="staffFormDialog.data"
      :lastId="staffList.length ? staffList[staffList.length - 1].id : 0"
      @create="onCreate"
      @edit="onEdit"
    ></staff-dialog>
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
import StaffDialog from "./staff-dialog.vue";
export default {
  components: {
    StaffDialog,
  },
  computed: {
    tableData() {
      return this.staffList.filter(
        (data) =>
          !this.search ||
          data.name.toLowerCase().includes(this.search.toLowerCase())
      );
    },
  },
  data() {
    return {
      staffFormDialog: {
        visible: false,
        data: {},
      },
      staffList: [
        {
          id: 1,
          name: "王小虎",
          salary: 8000,
          dept: "IT",
          position: "IT经理",
        },
        {
          id: 1,
          name: "李娟",
          salary: 8000,
          dept: "IT",
          position: "工程师",
        }
      ],
      search: "",
    };
  },
  methods: {
    onCreate(data) {
      this.staffList = [...this.staffList, data];
    },
    onEdit(data) {
      const index = this.staffList.findIndex((item) => item.id === data.id);
      this.staffList.splice(index, 1, data);
    },
    handleCreate() {
      this.staffFormDialog = {
        visible: true,
        data: {},
      };
    },
    handleEdit(row) {
      this.staffFormDialog = {
        visible: true,
        data: Object.assign({}, row),
      };
    },
    async handleDelete(index) {
      try {
        await this.$confirm("您是否确认删除？", "提示", { type: "warning" });
        this.staffList.splice(index, 1);
      } catch (err) {
        console.log(err);
      }
    },
  },
};
</script>
