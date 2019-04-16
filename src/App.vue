<template>
  <div id="app">
    <!-- <e-input v-model="defaultValue"></e-input>
    <div>
      {{ defaultValue }}
    </div> -->
    <e-form-item label="用户名" prop="name">
      <e-input v-model="ruleForm.name"></e-input>
    </e-form-item>
    <e-form-item label="密码" prop="pwd">
      <e-input v-model="ruleForm.pwd"></e-input>
    </e-form-item>
    <div>
      {{ ruleForm }}
    </div>
  </div>
</template>

<script>
import EInput from './components/Input'
import EFormItem from './components/FormItem'

export default {
  name: 'app',
  components: {
    EInput,
    EFormItem
  },
  data() {
    return {
      // defaultValue: 'abc'
      ruleForm: {
        name: '',
        pwd: ''
      }
    }
  },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
