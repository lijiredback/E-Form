<template>
  <div id="app">
    <!-- <e-input v-model="defaultValue"></e-input>
    <div>
      {{ defaultValue }}
    </div>-->
    <e-form :model="ruleForm" :rules="rules" ref="loginForm">
      <e-form-item label="用户名" prop="name">
        <div>
          <e-input v-model="ruleForm.name"></e-input>
        </div>
      </e-form-item>

      <e-form-item label="密码" prop="pwd">
        <e-input v-model="ruleForm.pwd"></e-input>
      </e-form-item>

      <e-form-item>
        <input type="submit" @click="submitForm()" value="提交">
      </e-form-item>

      <div>{{ ruleForm }}</div>
    </e-form>
  </div>
</template>

<script>
import EInput from "./components/Input";
import EFormItem from "./components/FormItem";
import EForm from "./components/Form";

export default {
  name: "app",
  components: {
    EInput,
    EFormItem,
    EForm
  },
  data() {
    return {
      // defaultValue: 'abc'
      ruleForm: {
        name: "",
        pwd: ""
      },
      rules: {
        name: [{ required: true }],
        pwd: [{ required: true }]
      }
    };
  },
  methods: {
    submitForm() {
      this.$refs.loginForm.validate(valid => {
        if (valid) {
          alert("登录成功");
        } else {
          alert('登录失败');
          // console.log("失败");
          return false;
        }
      });
      return false;
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
