<template>
  <v-form>
    <v-row>
      <!--Entry Price-->
      <v-col md="6">
        <v-text-field @input="priceChangeHandler" :prefix="type==='market' ? 'MKT' : ''" v-model="price"
                      label="entry price*" type="number"
                      suffix="USD"></v-text-field>
      </v-col>
      <!--Type-->
      <v-col md="4">
        <v-select @input="typeChangeHandler" v-model="type" label="Type" :items="typeOptions"></v-select>
      </v-col>
      <!--Place-->
      <v-col md="2">
        <v-checkbox v-model="place" label="Place"></v-checkbox>
      </v-col>
    </v-row>
    <v-row>
      <!--Take Profile Exit-->
      <v-col md="6">
        <v-text-field v-model="takeProfitExit" :rules="takeProfitRule" suffix="USD" type="number"
                      label="Take profit exit"></v-text-field>
      </v-col>
      <!--Gain %-->
      <v-col md="6">
        <v-text-field v-model="gain" suffix="%" label="%Gain"
                      prepend-inner-icon="mdi-approximately-equal"></v-text-field>
      </v-col>
    </v-row>
    <v-row>
      <!--Stop Loss-->
      <v-col md="6">
        <v-text-field v-model="stopLoss" :rules="stopLossRule" suffix="USD" type="number"
                      label="Stop Loss"></v-text-field>
      </v-col>
      <!--Loss %-->
      <v-col md="6">
        <v-text-field v-model="loss" suffix="%" label="%loss"
                      prepend-inner-icon="mdi-approximately-equal"></v-text-field>
      </v-col>
    </v-row>
    <slot name="amount_fields"></slot>
  </v-form>
</template>

<script>
export default {
  name: "Calculator",
  props: ['marketPrice'],
  data() {
    return {
      type: 'market',
      price: this.marketPrice,
      place: false,
      stopLoss: '',
      loss: '',
      gain: '',
      takeProfitExit: ''
    }
  },
  methods: {
    /**
     * handler for type select field
     */
    typeChangeHandler(val) {
      if (val === 'market') {
        this.price = this.marketPrice
      }
    },
    /**
     * handler for price field
     */
    priceChangeHandler() {
      this.type = 'limit';
    }
  },
  computed: {
    typeOptions() {
      return [
        {text: 'Limit', value: 'limit'},
        {text: 'Market', value: 'market'}
      ]
    },
    /**
     * validation rule for take profit
     * @returns {[]}
     */
    takeProfitRule() {
      return [
        v => v > this.marketPrice || 'Should be higher than market price'
      ]
    },
    /**
     * validation rule for stop loss
     * @returns {[]}
     */
    stopLossRule() {
      return [
        v => v > 0 || 'Price cannot be negative'
      ]
    }
  },
}
</script>

<style scoped>

</style>