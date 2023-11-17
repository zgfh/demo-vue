<template>
  <el-dialog
    v-bind="$attrs"
    v-on="$listeners"
    :title="dialogTitle"
    :close-on-click-modal="false"
    :close-on-press-escape="false"
    width="600px"
    append-to-body
    destroy-on-close
    @open="handleOpen"
    @closed="handleClosed"
  >
    <el-form
      ref="dialogForm"
      :model="formData"
      :rules="formRules"
      size="small"
      label-width="100px"
    >
      <el-form-item label="姓名：" prop="name">
        <el-input v-model="formData.name"></el-input>
      </el-form-item>
      <el-form-item label="年龄：" prop="age">
        <el-input v-model="formData.age"></el-input>
      </el-form-item>
      <el-form-item label="性别：" prop="sex">
        <el-radio-group v-model="formData.sex">
          <el-radio label="男"></el-radio>
          <el-radio label="女"></el-radio>
        </el-radio-group>
      </el-form-item>
      <el-form-item label="地址：" prop="address">
        <el-input type="textarea" v-model="formData.address"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button size="small" @click="handleCancel">取消</el-button>
        <el-button size="small" type="primary" @click="handleConfirm"
          >确定</el-button
        >
      </el-form-item>
    </el-form>
  </el-dialog>
</template>

<style lang="scss" scoped></style>

<script>
export default {
  props: {
    lastId: {
      type: Number,
      default: 0,
    },
  },
  data() {
    return {
      formData: {},
      formRules: {
        name: { required: true, message: "请输入姓名", trigger: "blur" },
        age: { required: true, message: "请输入年龄", trigger: "blur" },
        sex: { required: true, message: "请选择性别", trigger: "change" },
        address: { required: true, message: "请输入地址", trigger: "blur" },
      },
    };
  },
  computed: {
    isEdit() {
      return !!this.formData.id;
    },
    dialogTitle() {
      return this.isEdit ? "用户编辑" : "用户新增";
    },
  },
  methods: {
    // 点击保存
    async handleConfirm() {
      this.$refs["dialogForm"].validate((valid) => {
        if (valid) {
          this.handleSave();
        }
      });
    },
    // 创建||修改
    async handleSave() {
      if (this.isEdit) {
        this.$emit("edit", this.formData);
      } else {
        this.$emit("create", { id: this.lastId + 1, ...this.formData });
      }
      this.$emit("update:visible", false);
    },
    handleCancel() {
      this.$emit("update:visible", false);
    },
    handleOpen() {
      this.formData = this.$attrs.data;
    },
    handleClosed() {
      this.showLog = false;
      this.formData = {};
      this.$refs["dialogForm"].resetFields();
    },
  },
};
</script>
