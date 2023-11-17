<template>
  <el-tabs tab-position="left" style="height: 100vh">
    <el-tab-pane label="部门管理" lazy>
      <department-table></department-table>
    </el-tab-pane>
    <el-tab-pane label="员工管理" lazy>
      <employee-table></employee-table>
    </el-tab-pane>
    <el-tab-pane label="员工考核管理" lazy>
      <employeekpi-table></employeekpi-table>
    </el-tab-pane>
    <el-tab-pane label="数据分析管理" lazy>
      <salary-analyze-table></salary-analyze-table>
    </el-tab-pane>
  </el-tabs>
</template>

<script>
import DepartmentTable from '@/components/department-table.vue'
import EmployeeTable from '@/components/employee-table.vue'
import EmployeekpiTable from '@/components/employeekpi-table.vue'
import SalaryAnalyzeTable from '@/components/salary-analyze-table.vue'


export default {
  components: {
    DepartmentTable,
    EmployeeTable,
    EmployeekpiTable,
    SalaryAnalyzeTable
  }
}
</script>
