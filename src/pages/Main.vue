<template>
  <v-card 
    class="content"
    flat
  >
    <h1 class="title">Конвертер валют</h1>
    
    <div class="main">
      <div>
        <v-select
          @change="convert"
          v-model="fromCurrency"
          :items="currencyList"
          :loading="loading"
          hide-details
          outlined
          label="Меняю"
          class="main_select"
          color="#3b5b80"
          return-object
        >
        </v-select>

        <v-text-field
          @input="handleCount" 
          @keydown="isNumber"
          v-model="valueFromCurrency"
          hide-details
          type="number"
          class="main_select"
          color="#3b5b80"
          outlined
        >
        </v-text-field>
      </div>

      <div>
        <v-btn
          @click="rotate"
          icon
        >
          <v-icon 
            color="#3b5b80"
            x-large
          >
            mdi-swap-horizontal
          </v-icon>
        </v-btn>
      </div>

      <div>
        <v-select
          @change="convert"
          v-model="onCurrency"
          :items="currencyList"
          :loading="loading"
          hide-details
          outlined
          label="Получаю"
          class="main_select"
          color="#3b5b80"
          return-object
        >
        </v-select>

        <v-text-field 
          v-model="valueOnCurrency"
          :loading="loadingTextfield"
          hide-details
          class="main_select"
          readonly
          outlined
          color="#3b5b80"
          >
        </v-text-field>
      </div>
    </div>
  </v-card>  
</template>

<script>
  export default {
    data: () => ({
      currencyList: [],
      fromCurrency: null,
      onCurrency: null,
      valueFromCurrency: 1,
      valueOnCurrency: null,
      loading: false,
      loadingTextfield: false
    }),

    methods: {
      async fetchCurrencyList () {
        this.loading = true

        const url = `https://free.currconv.com/api/v7/currencies`
        const axios = require('axios')

        let currencyList = await axios.get(url, {
          params: {
            apiKey: '1fded8f792dcb451d100'
          }
        })

        currencyList = Object.values(currencyList.data.results)
        currencyList.forEach(el => {
          el.text = el.currencyName
          el.value = el.id
        })

        this.currencyList = currencyList
        this.loading = false
      },

      async convert () {
        const url = `https://free.currconv.com/api/v7/convert`
        const axios = require('axios')

        if (this.fromCurrency !== null && this.onCurrency !== null) {
          this.loadingTextfield = true
          let result = await axios.get(url, {
          params: {
            q: `${this.fromCurrency.value}_${this.onCurrency.value}`,
            compact: 'ultra',
            apiKey: '1fded8f792dcb451d100'
            }
          })

          this.valueOnCurrency = Object.values(result.data)[0]
          this.loadingTextfield = false
        }
      },

      async handleCount () {
        if (this.valueOnCurrency !== null) {
          await this.convert()
          this.valueOnCurrency = this.valueFromCurrency * this.valueOnCurrency 
        }
      },

      rotate () {
        const fromCurrency = this.fromCurrency
        const onCurrency = this.onCurrency
        this.onCurrency = fromCurrency
        this.fromCurrency = onCurrency
        this.handleCount()
      },

      isNumber (val) {
        return isNaN(Number(val.key)) ? false : true
      }
    },

    created () {
      this.fetchCurrencyList()
    }
  }
</script>

<style lang="scss" scoped>
  .content {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    max-width: 800px;
    width: 100%;
  }

  .title {
    text-align: center;
    font-size: 18px;
    margin-top: 4%;
    color: #3b5b80
  }

  .main {
    display: flex;
    justify-content: space-between;
    align-items: center;
    max-width: 700px;
    width: 100%;
    margin: 5% auto;

    &_select {
      width: 300px;
    }

    &_select:nth-child(2) {
      margin-top: 40px !important;
    }
  }
</style>
