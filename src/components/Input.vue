<template>
  <div>
    <!-- 
            1. bind value
            2. input events
    -->
    <input type="text" :value="valueInInput" @input="handleInput">
  </div>
</template>

<script>
export default {
  name: "EInput",
  props: {
    value: {
      type: String,
      default: ""
    }
  },
  data() {
    return {
      valueInInput: this.value // can not use value in this component
    };
  },
  methods: {
    handleInput(event) {
      this.valueInInput = event.target.value;
      // dispatch event
      this.$emit("input", this.valueInInput);

      // 数据变了，定向通知 FormItem 校验
      this.dispatch('EFormItem', 'validate', this.valueInput);
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
</style>