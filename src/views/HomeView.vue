<template>
  <main v-if="!loading">
      Show Data
  </main>

  <main v-else class="flex-col align-center justify-center text-center">
    <div class="text-gray-500 text-3xl mt-10 mb-6">
      Fetching Data
    </div>
    <img :src="loadingImage" alt="" class="w-24 m-auto">
  </main>
</template>

<script>
// @ is an alias to /src

export default {
  name: 'HomeView',
  components: {
  },
  data () {
    return {
      loading: true,
      title: 'Global',
      dataDate: '',
      status: {},
      countries: [],
      loadingImage: require('../assets/hourglass.gif')
    }
  },
  methods: {
    async fetchCovidData () {
      const res = await fetch('https://api.covid19api.com/summary')
      const data = await res.json()
      return data
    }
  },
  async created () {
    const data = await this.fetchCovidData()
    console.log(data)

    this.dataDate = data.Date
    this.stats = data.Globle
    this.countries = data.Countries
    this.loading = false
  }
}
</script>
