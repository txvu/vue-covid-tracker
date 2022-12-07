<template>
  <main v-if="!loading">
    <DataTitleComp :text="title" :dataDate="dataDate"/>
    <DataBoxesComp :stats="stats"/>
    <CountrySelectComp @get-country="getCountryData" :countries="countries"/>
    <button @click="clearCountryData" v-if="stats.Country" class="bg-green-700 text-white rounded p-3 mt-10 focus:outline-none hover:bg-green-600">Clear Country</button>
  </main>

  <main v-else class="flex-col align-center justify-center text-center">
    <div class="text-gray-500 text-3xl mt-10 mb-6">
      Fetching Data
    </div>
    <img :src="loadingImage" alt="" class="w-24 m-auto">
  </main>
</template>

<script>
import DataTitleComp from '@/components/DataTitleComp.vue'
import DataBoxesComp from '@/components/DataBoxesComp.vue'
import CountrySelectComp from '@/components/CountrySelectComp.vue'

export default {
  name: 'HomeView',
  components: {
    DataTitleComp,
    DataBoxesComp,
    CountrySelectComp
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
    },
    getCountryData (country) {
      this.stats = country
      this.title = country.Country
    },
    async clearCountryData () {
      this.loading = true
      const data = await this.fetchCovidData()
      this.dataDate = data.Date
      this.title = 'Global'
      this.stats = data.Global
      this.countries = data.Countries
      this.loading = false
    }
  },
  async created () {
    const data = await this.fetchCovidData()
    console.log(data)

    this.dataDate = data.Date
    this.stats = data.Global
    this.countries = data.Countries
    this.loading = false
  }
}
</script>
