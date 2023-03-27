<template>
  <div class="home">
    <el-form
      ref="ruleFormRef"
      :model="ruleForm"
      status-icon
      :rules="rules"
      label-width="120px"
      class="demo-ruleForm"
    >
      <el-form-item label="账号" prop="account">
        <el-input v-model="ruleForm.account" autocomplete="off" />
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input
          v-model="ruleForm.password"
          type="password"
          autocomplete="off"
        />
      </el-form-item>

      <el-form-item>
        <el-button type="primary" @click="submitForm(ruleFormRef)"
          >Submit</el-button
        >
        <el-button @click="resetForm(ruleFormRef)">Reset</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, toRefs, ref } from "vue";
import { lfData } from "./type";
import type { FormInstance, FormRules } from "element-plus";
export default defineComponent({
  name: "HomeView",
  components: {},
  setup() {
    const ruleFormRef = ref<FormInstance>();
    const data = reactive({
      ruleForm: new lfData(),
      rules: {
        account: [
          {
            required: true,
            message: "Please input account",
            trigger: "blur",
          },
          {
            min: 3,
            max: 5,
            message: "Length should be 3 to 5",
            trigger: "blur",
          },
        ],
        password: [
          {
            required: true,
            message: "Please input password",
            trigger: "blur",
          },
          {
            min: 3,
            max: 5,
            message: "Length should be 3 to 5",
            trigger: "blur",
          },
        ],
      },
    });

    const submitForm = async (formEl: FormInstance | undefined) => {
      if (!formEl) return;
      await formEl.validate((valid, fields) => {
        if (valid) {
          console.log("submit!");
        } else {
          console.log("error submit!", fields);
        }
      });
    };
    const resetForm = (formEl: FormInstance | undefined) => {
      if (!formEl) return;
      formEl.resetFields();
    };

    return {
      ruleFormRef,
      ...toRefs(data),
      submitForm,
      resetForm,
    };
  },
});
</script>

<style lang="less" scoped>
</style>
