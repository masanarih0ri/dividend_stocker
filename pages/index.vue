<template>
  <div class="container">
    <ul>
      <li v-for="(result, index) in results" :key="index">
        {{result.ticker}} {{result.currentStockPrice}} {{result.dividendYield}} {{result.yieldAfterTax}}
      </li>
    </ul>
    {{dividendPerYear}}
  </div>
</template>

<script>

export default {
  data() {
    return {
      results: []
    }
  },
  async asyncData({ app }) {
    const baseUrl = 'https://api.steinhq.com/v1/storages/5e2ff4245a823204986f3ae8/'
    const sheetName = 'stocksData'
    const getUrl = encodeURI(baseUrl + sheetName)
    const response = await app.$axios.$get(getUrl)
    return {
      results: response
    }
  },
  computed: {
    dividendPerYear() {
      return this.results.map(x => x.currentStockPrice * x.numbersOfSharesHeld * x.yieldAfterTax / 100)
    }
  }
}
</script>

<style>

</style>
