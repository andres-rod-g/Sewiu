<template>
    <div id="currency-container">
        <h2 for="currency">Currency</h2>
        <select name="select" id="curr-select" @change="updateCurrency" v-model="currency">
            <option value="COP">COP</option>
            <option value="USD">USD</option>
            <option value="EUR">EUR</option>
            <option value="ARS">ARS</option>
        </select>
    </div>
</template>

<script>
export default {
    name: 'FormCurrency',
    props: [
        'currency_prop'
    ],
    methods: {
        updateCurrency(event) {
            console.log(event.target.value)
            this.$emit('updateCurrency', event.target.value)
        }
    },
    data() {
        return {
            currency: this.currency_prop
        }
    }
};
</script>

<style scoped>
    #currency-container {
        display: flex;
        flex-direction: column;
    }

    select {
        padding: 10px;
        border: 1px solid #23232328;
        border-radius: 10px;
    }
</style>
