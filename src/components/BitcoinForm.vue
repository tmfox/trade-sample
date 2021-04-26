<template>
  <div class="blue-grey darken-4">
    <!-- Header -->
    <div class="d-flex align-center">
      <v-tabs grow background-color="transparent">
        <!-- Buy Button -->
        <v-tab @click="formType='buy'" active-class="green--text">Buy BTC</v-tab>
        <!--Slider Color-->
        <v-tabs-slider :color="formType==='buy'? 'green' : 'red'"></v-tabs-slider>
        <!--Sell Button-->
        <v-tab @click="formType='sell'" active-class="red--text">Sell BTC</v-tab>
      </v-tabs>
      <!--Calculator Button-->
      <v-btn @click="showCalculator = !showCalculator" icon>
        <!-- Calculator Icon -->
        <v-icon v-if="!showCalculator">mdi-calculator</v-icon>
        <!-- Back Icon -->
        <v-icon v-else>mdi-rotate-left</v-icon>
      </v-btn>
    </div>
    <!-- Form -->
    <v-container v-show="showCalculator===false" fluid>
      <v-form>
        <v-row>
          <v-col md="6">
            <!-- Price Field -->
            <v-text-field v-model="price" :disabled="order_type==='market_order'" color="blue" hide-details
                          :type="order_type==='limit_order' ? 'number' : 'text'" suffix="USD"
                          label="Price" :readonly="order_type==='market_order'"></v-text-field>
          </v-col>
          <v-col>
            <!--order type select-->
            <v-select @change="orderTypeHandler" v-model="order_type" color="blue" label="Order Type"
                      :items="orderTypeOptions"></v-select>
          </v-col>
        </v-row>
        <v-row>
          <!-- Amount in BTC -->
          <v-col md="6">
            <v-text-field :rules="amountRule" v-model="btc_amount" type="number" suffix="BTC"
                          label="Amount"></v-text-field>
          </v-col>
          <!--Amount in USD-->
          <v-col md="6">
            <v-text-field :rules="amountRule" v-model="usd_amount"
                          :prepend-inner-icon="btc_amount>0.0001 ? 'mdi-approximately-equal' : ''" color="blue"
                          hide-details type="number" suffix="USD"
                          label="Amount"></v-text-field>
          </v-col>
        </v-row>
        <!--Order Type Post|Ioc-->
        <div class="d-flex">
          <v-switch v-model="orderType2" value="post" hide-details label="POST"></v-switch>
          <v-switch v-model="orderType2" value="ioc" class="ml-8" hide-details label="IOC"></v-switch>
        </div>
        <v-btn class="mt-4" block color="light-blue white--text">Execute</v-btn>
      </v-form>
    </v-container>
    <v-container v-show="showCalculator" fluid>
      <Calculator :market-price="market_price">
        <!-- Amount Fields -->
        <template slot="amount_fields">
          <v-row>
            <!--Duplicate of BTC Amount Field-->
            <v-col md="6">
              <v-text-field v-model="btc_amount" type="number" suffix="BTC"
                            label="Amount"></v-text-field>
            </v-col>
            <!-- Duplicate of USD Amount Field -->
            <v-col md="6">
              <v-text-field v-model="usd_amount" color="blue" hide-details type="number" suffix="USD"
                            label="Amount"></v-text-field>
            </v-col>
          </v-row>
        </template>
      </Calculator>
    </v-container>
  </div>
</template>

<script>
import Calculator from "./Calculator";

export default {
  name: "BitcoinForm",
  data() {
    return {
      formType: 'buy',
      order_type: 'limit_order',
      price: '',
      market_price: 50250,
      usd_amount: '',
      btc_amount: '',
      orderType2: '',
      showCalculator: false
    }
  },
  components: {Calculator},
  computed: {
    orderTypeOptions() {
      return [
        {text: 'Limit Order', value: 'limit_order'},
        {text: 'Market Order', value: 'market_order'},
      ]
    },
    amountRule() {
      return [
        v => v > 0.0001 || 'Amount must be greater than 0.0001'
      ]
    }
  },
  methods: {
    orderTypeHandler(val) {
      if (val === 'market_order') {
        this.price = 'MARKET'
      }
    }
  }
}
</script>

<style scoped>
</style>