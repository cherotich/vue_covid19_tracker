<template>
<div class="grid md:grid-cols-2 gap-4">
   <!-- Box1 -->
    <div class="shadow-md bg-blue-100 p-10 text-center rounded">
        <h3 class="text-3xl text-blue-900 font-bold mb-4">
            Cases
        </h3>
        <div class="text-2xl mb-4">
          <span class="font-bold">New:</span>
         
           {{ stats.NewConfirmed }}       
        </div>
          <div class="text-2xl mb-4">
  <span class="font-bold">
              Total: 
          </span>
          {{ stats.TotalConfirmed }}
          </div>
        
    </div>
    <!-- Box2 -->
        <div class="shadow-md bg-blue-200 p-10 text-center rounded">
        <h3 class="text-3xl text-blue-900 font-bold mb-4">
            Cases
        </h3>
        <div class="text-2xl mb-4">
          <span class="font-bold">New:</span>
         
           {{ stats.NewDeaths }}       
        </div>
          <div class="text-2xl mb-4">
  <span class="font-bold">
              Total: 
          </span>
          {{ stats.TotalDeaths}}
          </div>
        
    </div>
</div>
</template>
<script>
export default {
    name: 'DataBoxes',
    props: ['stats'],
    methods: {
        numberWithCommas(x){
             return x.toString()
          .replace(/\B(?=(\d{3})+(?!\d))/g, ',');
            }
    }
    }
</script>



<template>
  <main v-if="!loading">
  <DataTitle :text="title" :dataDate="dataDate"/>
  <!-- <DataBoxes :stats="stats"/> -->
  <CountrySelect @get-country="getCountryData" :countries="countries" />
  </main>
  <main class="flex flex-col align-center justify-center text-center" v-else>
 <div class ="text-gray-500 text-3xl mt-10 mb-6">
   Fetching data
 </div>
 <img :src="loadingImage" class="w-24 m-auto" alt=""/>
  </main>
</template>

<script>
import DataTitle from '@/components/DataTitle'
import DataBoxes from '@/components/DataBoxes'
import CountrySelect from '@/components/CountrySelect'

// import DataTitle from '../components/DataTitle.vue'
export default {
  name: 'Home',
  components: { 
    DataTitle,
    DataBoxes,
    CountrySelect
   },
    DataTitle() {
    return {
      loading:true,
      title:'Clobal',
      dataDate:'',
      stats:{},
      countries:[],
      loadingImage:require('../assets/hourglass.gif'),

    }
    },
  methods:{
  async  fetchCovidData() {
    const res = await fetch('https://api.covid19api.com/summary')
    const data = await res.json()
    return data
      },
      getCountryData(country) {

        }
    },
 async created(){
    const data  = await  this.fetchCovidData()
    this.dataDate = data.Date
    this.stats = data.Global
    this.countries = data.Countries
    this.loading = false

    console.log(this.stats)
},
}
</script>