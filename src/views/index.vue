<template>
  <el-tabs tab-position="left" style="height: 100vh">
    <el-tab-pane label="部门管理" lazy>
      <department-table></department-table>
    </el-tab-pane>
    <el-tab-pane label="员工管理" lazy>
      <staff-table></staff-table>
    </el-tab-pane>
  </el-tabs>
</template>

<script>
import DepartmentTable from '@/components/department-table.vue'
import StaffTable from '@/components/staff-table.vue'

export default {
  components: {
    DepartmentTable,
    StaffTable
  }
}
</script>
