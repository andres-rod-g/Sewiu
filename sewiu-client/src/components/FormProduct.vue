<template>
    <tr>
        <td>{{selectedProduct.sku}}</td>
        <td>
            <select v-model="selectedProduct" @change="changeSelected">
                <option v-for="prod in products_fetched" :key="prod.sku" :value="prod">{{prod.name}}</option>
            </select>
        </td>
        <td>
            <div id="units">
                <button @click="decreseUnit" class=".noselect">-</button>
                <input type="number" min="1" @change="handleUnits" v-model="units">
                <button @click="addUnit" class=".noselect">+</button>
            </div>
        </td>
        <td>{{selectedProduct.currency + ' ' + selectedProduct.price}}</td>
        <td>{{selected_currency_info.code + conversion}}</td>
        <td>{{selected_currency_info.code + quantity_price}}</td>
        <td><input type="number" min="0" max="100" placeholder="0" @change="updateData" v-model="discount"></td>
        <td>{{selectedProduct.taxRate + '%'}}</td>
        <td>{{selected_currency_info.code + ' ' + sub_total}}</td>
    </tr>
</template>

<script>
import "vue-select/dist/vue-select.css";


export default {
    name: "FormProduct",
    props: [
        'product_prop',
        'products_fetched',
        'selected_currency_info',
        'currency_fetched',
    ],
    components: {
    },
    methods: {
        findMyCurrency(currency_code) {
            for (let i = 0; i < this.currency_fetched.length; i++){
                console.log(this.currency_fetched[i])
                if (this.currency_fetched[i].code == currency_code) {
                    return this.currency_fetched[i]
                }
            }
        },
        updateData(){
            if (this.currency == this.selected_currency_info.code){
                this.conversion = this.selectedProduct.price
                this.quantity_price = ' ' + parseFloat(this.units * this.selectedProduct.price).toFixed(2)
            } else {
                const thisCurrency = this.findMyCurrency(this.selectedProduct.currency)
                console.log(thisCurrency, this.selectedProduct.price, this.currency_price, thisCurrency.value)
                this.conversion = ' ' + parseFloat((this.selectedProduct.price * this.currency_price) / thisCurrency.value).toFixed(2)
                this.quantity_price = ' ' + parseFloat(((this.selectedProduct.price * this.currency_price) / thisCurrency.value) * this.units).toFixed(2)
            }

            const val = Number(Number(this.quantity_price)  - (Number(this.quantity_price) * Number(this.discount) / 100))

            console.log(val)
            console.log(val + Number(Number(val) * Number(this.selectedProduct.taxRate) / 100))
            
            this.sub_total = parseFloat((val + (Number(val) * Number(this.selectedProduct.taxRate) / 100))).toFixed(2)

            this.$emit("changedSubTotal", {"uuid": this.product_prop.entry_uuid, "sub_total": Number(this.sub_total)})

        },
        addUnit(e) {
            e.preventDefault()
            this.units += 1

            this.updateData()
         },
        decreseUnit(e) {
            e.preventDefault()

            if (this.units > 1) {
                this.units -= 1
            } else {
                //Remove Product
                console.log('about to remove')
                this.$emit('removeProduct', this.product_prop.entry_uuid)
            }

            this.updateData()
        },
        changeSelected() {
            console.log(this.selectedProduct.currency)
            this.currency = this.selectedProduct.currency
            this.updateData()
        },
        handleUnits(e) {
            if (e.target.value < 1) {
                this.$emit('removeProduct', this.product_prop.entry_uuid)
            }
        }
    },
    data() {
        return {
            units: 1,
            discount: 0,
            currency: 'USD',
            currency_price: 0,
            conversion: 0,
            quantity_price: 0,
            sub_total: 0,
            selectedProduct:  
          {
            "sku": "--",
            "name": "Pinzas",
            "price": 0,
            "currency": "USD",
            "taxRate": 0
          },
        }
    },
    watch: {
        selected_currency_info: {
            immediate: true,
            handler (val) {
                this.currency_price = val.value 

                console.log(this.currency, val.code)
                this.updateData()

            }
        },
    },
}
</script>

<style scoped>

    input::-webkit-outer-spin-button,
    input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
    }

    /* Firefox */
    input[type=number] {
    -moz-appearance: textfield;
    }


    #units {
        display: flex;
        flex-direction: row;
        align-items: center;
        justify-content: space-between;
    }

    td, table, th {
        border-radius: 10px;
        padding: 5px;
        background: whitesmoke;
        height: 50px;
    }

    button {
        background: #136dff;
        border: 0px;
        border-radius: 10px;
        height: 18px;
        width: 18px;
        display: flex;
        align-items: center;
        justify-content: center;
        color: rgb(255, 255, 255);
        cursor: pointer;
    }

    input {
        width: 50%;
        border: 1px solid #2323234d;
        border-radius: 10px;
        padding: 5px;
        text-align: center;
    }

    select {
        width: 80%;
        border: 1px solid #2323234d;
        border-radius: 10px;
        padding: 5px;
        text-align: left;
    }
</style>