<template>
  <div>
    <label v-if="label">{{ label }}</label>
    <div>
      <!-- put sth here replace the slot element: like input element -->
      <slot></slot>
      <p v-if="validateState === 'error' " class="error">{{ validateMessage }}</p>
    </div>
  </div>
</template>

<script>
import AsyncValidator from "async-validator";

export default {
  name: "EFormItem",
  props: {
    label: { type: String, default: "" },
    prop: { type: String, default: "" }
  },
  inject: ["eForm"],
  created() {
    this.$on("validate", this.validate);
  },
  mounted() {
    // 挂载到 form 上时，派发一个添加事件
    // 告诉 form, 我已经成功的挂载上了，你可以用我了
    // formItem 中，没有 prop 属性的，不需要校验
    if (this.prop) {
      // this.$parent.$emit("EForm", this);
      this.dispatch('EForm', 'addFiled', this);
    }
  },
  data() {
    return {
      validateMessage: "",
      validateState: ""
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
          // name: this.form.rules.name =>
          // name: [ { require: true }, { ... } ]
        };
        descriptor[this.prop] = this.eForm.rules[this.prop];
        // 校验器
        const validator = new AsyncValidator(descriptor);
        const model = {};
        model[this.prop] = this.eForm.model[this.prop];
        // 异步校验
        validator.validate(model, errors => {
          if (errors) {
            // 校验失败
            // 设置校验状态 和 错误信息
            this.validateState = "error";
            this.validateMessage = errors[0].message;

            resolve(false);
          } else {
            this.validateState = "";
            this.validateMessage = "";

            resolve(true);
          }
        });
      });
    },
    // 查找上级元素
    dispatch(componentName, eventName, params) {
      var parent = this.$parent || this.$root;
      var name = parent.$options.name;

      while (parent && (!name || name !== componentName)) {
        parent = parent.$parent;

        if (parent) {
          name = parent.$options.name;
        }
      }
      if (parent) {
        parent.$emit.apply(parent, [eventName].concat(params));
      }
    }
  }
};
</script>

<style scoped>
.error {
  color: red;
}
</style>