<template>
    <div id="changerate-container">
        <h2>Exchange Rate (USD)</h2>
        <input type="number" @change="changeRate" v-model="currency_info.value" v-bind:disabled="currency_info.code == 'USD' ? true : null">
    </div>
</template>

<script>
export default {
    name: 'FormChangeRate',
    props: [
        'selected_currency_info'
    ],
    methods: {
        changeRate (e) {
            console.log(e.target.value)
            this.$emit('changeRate', e.target.value)
        }
    },
    data() {
        return {
            currency_info: this.selected_currency_info,
        }
    },
    watch: {
        //Keep track of selected currency, to know if it's changed.
        selected_currency_info: {
            immediate: true,
            handler (val) {
                this.currency_info = val
            }
        }
    }
}
</script>

<style scoped>
    #changerate-container {
        width: 100%;
        display: flex;
        flex-direction: column;
        justify-items: flex-end;
        align-items: flex-end;
    }
</style>