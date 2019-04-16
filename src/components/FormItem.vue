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
    export default {
        props: [ 'label', 'prop' ],
        data() {
            return {
                errorMessage: '',
                validateStatus: '',
            }
        },
    }
</script>

<style scoped>
    .error{
        color: red;
    }
</style>