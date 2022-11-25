<template>
    <h2>Products</h2>
    <div id="container">
        <div id="product-container">
            <table >
                <col style="width: 5%">
                <col style="width: 25%">
                <col style="width: 10%">
                <col style="width: 10%">
                <col style="width: 10%">
                <col style="width: 10%">
                <col style="width: 10%">
                <col style="width: 10%">
                <col style="width: 10%">
                <thead>
                    <tr>
                        <td colspan="9" style="text-align: center; cursor: pointer; background: #136dff; color: white;" class=".noselect" @click="addProduct"><b>+</b></td>
                    </tr>
                    <tr id="header">
                        <th>SKU</th>
                        <th>Product</th>
                        <th>Units</th>
                        <th>Unit / Price</th>
                        <th>Unit Conversion</th>
                        <th>Cuantity Price</th>
                        <th>Discount - %</th>
                        <th>Tax - %</th>
                        <th>SubTotal</th>
                    </tr>
                </thead>
                <tbody v-for="product in products" :key="product.entry_uuid">
                    <FormProduct v-bind:products_fetched="products_fetched" v-bind:product_prop="product" v-on:removeProduct="$emit('removeProduct', product.entry_uuid)" v-bind:selected_currency_info="selected_currency_info" v-bind:currency_fetched="currency_fetched" v-on:changedSubTotal="changedSubTotal"/>
                </tbody>
            </table>
        </div>
        <div id="total-viewer">
            <div class="highlighted">
                <b>Total</b>
            </div>
            <div class="highlighted">
                {{this.selected_currency_info.code + ' ' + this.total}}
            </div>
        </div>
    </div>
</template>

<script>
import FormProduct from './FormProduct.vue'
import {uuid} from 'vue-uuid'

export default {
    name:'FormProducts',
    components: {
        FormProduct
    },
    props: [
        'products',
        'products_fetched',
        'selected_currency_info',
        'currency_fetched',
    ],
    data () {
        return {
            total: 0,
        }
    },
    methods: {
        addProduct(e) {
            e.preventDefault()

            const newProduct = {
                entry_uuid: uuid.v4(),
                sub_total: 0,
            }

            this.$emit('addProduct', newProduct)
        },
        changedSubTotal(e) {
            this.$emit('changedSubTotal', e)
        }
    },
    watch: {
        products: {
            handler () {
                let newTotal = (this.products.reduce((partialSum, a) => partialSum + a.sub_total, 0))
                console.log(newTotal)
                console.log(newTotal)
                console.log(newTotal)
                this.total = (newTotal).toLocaleString()
            },
            deep: true,
        }
    }
}
</script>

<style scoped>

    ::-webkit-scrollbar {
    width: 8px;
    }

    /* Track */
    ::-webkit-scrollbar-track {
    background: #f1f1f1;
    border-radius: 10px;
    }

    /* Handle */
    ::-webkit-scrollbar-thumb {
        border-radius: 10px;
    background: #888;
    }

    /* Handle on hover */
    ::-webkit-scrollbar-thumb:hover {
        border-radius: 10px;
    background: #555;
    }
    #container {
        display: flex;
        flex-direction: column;
        width: 100%;
    }
    #product-container {
        display: flex;
        flex-direction: column;
        width: 100%;
        border-radius: 10px;
        background: rgb(240, 240, 240);
        height: 300px;
        overflow-x: scroll;
        overflow-y: auto;
        -layout: fixed;
        padding: 5px;
    }

    #total-viewer {
        display: flex;
        flex-direction: row;
        position: relative;
        align-self: flex-end;
        right: 0;
        bottom: 0;
        padding: 20px;
        margin-top: 10px;
        background: rgb(240, 240, 240);
         border-radius: 10px;
    }
    thead {
        text-align: left;
        top: 0;
        position: sticky;
        background-color: white;
        text-align: center;
    }

    .highlighted {
        text-align: left;
        top: 0;
        position: sticky;
        background-color: white;
        padding: 10px;
        margin: 5px;
        border-radius: 10px;
    }

    td, table, th {
        border-radius: 10px;
        padding: 5px;
    }

    button {
        width: 100%;
        border: 0px;
        padding: 10px;
        cursor: pointer;
        position: -webkit-sticky;
        position:sticky;
        top: -1;
    }

    h2 {
        text-align: left;
    }

</style>