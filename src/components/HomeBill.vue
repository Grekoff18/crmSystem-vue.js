<template>
  <div class="col s12 m6 l4">
    <div class="card light-blue bill-card">
      <div class="card-content white-text">
        <span class="card-title">Currency account</span>

        <p
          class="currency-line"
          v-for="cur of currencies"
          :key="cur"
        >
          <span>{{getCurrency(cur) | currency(cur)}}</span>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["rates"],

  data: () => {
    return {
      currencies: ["USD", "EUR", "UAH"]
    }
  },

  computed: {
		// Compute base bill whose we have 
    base() {
      return this.$store.getters.info.bill / (this.rates["UAH"] / this.rates["EUR"])
    }
  },

  methods: {
		// Computing our bill in other currencies
    getCurrency(currency) {
      return Math.floor(this.base * this.rates[currency])
    }
  }
}
</script>