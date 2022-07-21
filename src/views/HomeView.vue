<template>
  <main v-if="!loading">
    <DataTitle :text="title" :dataDate="dataDate"/>
    <DataBoxes :stats="stats"/>
    <CountrySelect @get-country="getCountryData" :countries="countries"/>
    <!-- <button
      v-if="status.Country"
      class="bg-green-700 text-white rounded p-3 mt-10 focus:outline-none hover:bg-green-600"
      @click="clearCountryData"
    >
      Clear Country
    </button> -->
  </main>

  <main v-else class="flex flex-col align-center justify-center text-center">
    <div class="text-gray-500 text-3xl mt-10 mb-6">
      Fetching Data
    </div>
    <img :src="require('../assets/hourglass.gif')" alt="" class="w-24 m-auto" />
  </main>
</template>

<script>

import DataTitleVue from '@/components/DataTitle.vue'
import DataTitle from '@/components/DataTitle.vue'
import DataBoxesVue from '@/components/DataBoxes.vue'
import DataBoxes from '@/components/DataBoxes.vue'
import CountrySelectVue from '@/components/CountrySelect.vue'
import CountrySelect from '@/components/CountrySelect.vue'
export default {
  name: 'HomeView',
  components: {
    DataTitleVue,
    DataTitle,
    DataBoxesVue,
    DataBoxes,
    CountrySelectVue,
    CountrySelect
},
  data() {
    return {
      loading: true,
      title: "Global",
      dataDate: '',
      stats: {},
      countries: [],
      loadingImage: require('../assets/hourglass.gif')

    }
  },
  methods:{
     async fetchCovidData(){
      const res = await fetch('https://api.covid19api.com/summary')
      const data = await res.json()
      return data
    },
    getCountryData(country){
      this.stats = country
      this.title = country.Country
      
    },
    // async clearCountryData(){
    //   this.loading = true
    //   const data = await this.fetchCovidData()
    //   this.title = "Global"
    //   this.stats = data.Global
    //   this.loading = false
    // }
  },
  async created(){
    const data = await this.fetchCovidData()
    this.dataDate = data.Date
    this.stats = data.Global
    this.countries = data.Countries
    this.loading = false
  },
}
</script>
