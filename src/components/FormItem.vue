<template>
  <div>
    <label v-if="label">{{ label }}</label>
    <div>
      <!-- put sth here replace the slot element: like input element -->
      <slot></slot>
      <p v-if="validateStatus === 'error' " class="error">{{ errorMessage }}</p>
    </div>
  </div>
</template>

<script>
import schema from "async-validator";

export default {
  props: ["label", "prop"],
  inject: ["form"],
  created() {
    this.$on("validate", this.validate);
  },
  mounted() {
    // 挂载到 form 上时，派发一个添加事件
    // 告诉 form, 我已经成功的挂载上了，你可以用我了
    // formItem 中，没有 prop 属性的，不需要校验
    if (this.prop) {
      this.$parent.$emit("formItemAdd", this);
    }
  },
  data() {
    return {
      errorMessage: "",
      validateStatus: ""
    };
  },
  methods: {
    validate() {
        // 返回一个 promise 对象，批量处理所有异步结果
      return new Promise(resolve => {
        // 检查当前项，依赖 async-validator
        // name: {
        //     type: "string",
        //     required: true,
        //     validator: (rule, value) => value === 'muji',
        // },
        const descriptor = {
          [this.prop]: this.form.rules[this.prop]
          // name: this.form.rules.name =>
          // name: [ { require: true }, { ... } ]
        };
        // 校验器
        const validator = new schema(descriptor);
        // 异步校验
        validator.validate({ [this.prop]: this.form.model[this.prop] }, errors => {
          if (errors) {
            // 校验失败
            // 设置校验状态 和 错误信息
            this.validateStatus = "error";
            this.errorMessage = errors[0].message;

            resolve(false);
          } else {
            this.validateStatus = "";
            this.errorMessage = "";

            resolve(true);
          }
        });
      });
    }
  }
};
</script>

<style scoped>
.error {
  color: red;
}
</style>