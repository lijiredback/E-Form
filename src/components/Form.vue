<template>
  <form onsubmit="return false">
    <slot></slot>
  </form>
</template>

<script>
    export default {
        name: 'EForm',
        props: {
            model: { type: Object, required: true },
            rules: { type: Object }
        },
        provide() {
          return {
              eForm: this, // provide this component's instance
          }  
        },
        data() {
            return {
                fileds: [],
            }
        },
        created() {
            // 缓存需要校验的表单项
            this.fileds = [];
            this.$on('addFiled', filed => this.fileds.push(filed));
        },
        methods: {
            async validate(cb) {
                // 将 FormItem 数组转换为 validate() 返回的 promise 数组
                const tasks = this.fileds.map(item => item.validate());
                // console.log(tasks);

                // 获取所有结果统一处理
                const results = await Promise.all(tasks);
                let ret = true;
                results.forEach(valid => {
                    if (!valid) {
                        ret = false; // 只要有一个失败就失败
                    }
                });
                cb(ret);
            }
        },
    }
</script>

<style lang="scss" scoped>
</style>