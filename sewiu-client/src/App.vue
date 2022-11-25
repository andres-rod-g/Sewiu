<template>
    <h1>Sewiu - Andrés Rodríguez</h1>
    <div id="form-container" action="">
        <div id="first-container">
            <div id="second-container">
              <FormTitle />
              <FormCurrency v-bind:currency_prop="currency" v-on:updateCurrency="updateCurrency"/>
            </div>
            <FormChangeRate v-model:selected_currency_info="selected_currency_info" v-on:changeRate="changeRate"/>
        </div>
        <FormProducts v-on:addProduct="addProduct" v-bind:products_fetched="products_fetched" v-bind:currency_fetched="currency_fetched" v-bind:products="products" v-bind:selected_currency_info="selected_currency_info" v-on:removeProduct="removeProduct" v-on:changedSubTotal="changedSubTotal"/>
    </div>
</template>

<script>
import FormTitle from "./components/FormTitle.vue";
import FormCurrency from './components/FormCurrency.vue'
import FormChangeRate from './components/FormChangeRate.vue'
import FormProducts from './components/FormProducts.vue';

//Tool used for fetching.
import axios from 'axios'

export default {
    name: "App",
    components: {
        FormTitle,
        FormCurrency,
        FormChangeRate,
        FormProducts,
    },
    methods: {


      addProduct (product) {
        //Add Product to the list of products that are registered on the table.
        this.products.push(product)
      },


      removeProduct(uuid){
        //Remove product from the list of products of the table.
        this.products = this.products.filter((product) => product.entry_uuid != uuid)
      },


      updateCurrency(currency) {
        //When changed the currency, 
        //we will update it to all the components.
        this.currency = currency

        const filtered = this.currency_fetched.filter((curr) => curr.code == currency)
        this.selected_currency_info = {...filtered[0]}
        console.log(this.selected_currency_info)
      },


      changeRate(value) {
        //When changed the value of the exchange rate, 
        //we will update it to all the components.
        this.selected_currency_info = {...this.selected_currency_info, value}
      },



      changedSubTotal(value){
        //When changed the SubTotal in any of the products, we will update it everywhere.
        for (let i = 0; i < this.products.length; i++) {
          if (this.products[i].entry_uuid == value.uuid) {
            this.products[i].sub_total = value.sub_total
            console.log(this.products[i])
          }
        }
      }



    },
    data () {
      return {
        currency: 'COP',
        selected_currency_info: {
        },
        currency_fetched: [
        ],
        products_fetched: [
        ],
        products: [],
      }
    },
    mounted () {
      // When mounted, We're going to fetch data from the API

      let API_URL = 'http://localhost:3000/api/'

      // Products
      axios
        .get(API_URL + 'products')
        .then(response => {this.products_fetched = response.data; console.log(response)})

      // Currency
      axios
        .get(API_URL + 'currency')
        .then(response => {
          this.currency_fetched = response.data
          let cop = response.data.filter(object => object.code == 'COP')
          this.selected_currency_info = cop[0]
        })
    }
};
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
#form-container {
    display: flex;
    flex-direction: column;
    width: 80vw;
    justify-content: center;
    justify-self: center;
    align-items: center;
    margin: auto;
}
#first-container {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
    width: 50%;
    min-width: 350px;
}

  #second-container {
    display: flex;
    flex-direction: row;
    width: 100%;
    justify-content: space-between;
  }
</style>
