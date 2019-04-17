<template>
    <div>
        <!-- 
            1. bind value
            2. input events
         -->
        <input type="text" :value="inputValue" @input="inputHandler">
    </div>
</template>

<script>
    export default {
        props: {
            value: {
                type: String,
                default: ''
            }
        },
        data() {
            return {
                inputValue: this.value // can not use value in this component
            }
        },
        methods: {
            inputHandler(e) {
                this.inputValue = e.target.value;
                // dispatch event
                this.$emit('input', this.inputValue);

                // 数据变了，定向通知 FormItem 校验
                this.$parent.$emit('validate', this.inputValue); // todo $parent 不通用
            }
        },
    }
</script>

<style scoped>

</style>