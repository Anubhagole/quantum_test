<template>
<div>
  <b-dropdown id="dropdown-1" :text="country.name" class="m-md-2">

    <b-dropdown-item v-for="(country, index) in countries" :key="index" @click="getCurrency(country); getState()">{{(country.name)}}
    
    </b-dropdown-item>
  </b-dropdown>
  <h3>{{country.name}}</h3>
  <h3>ISO Code:</h3> <p>{{country.Iso2}}</p><p>{{country.Iso3}}</p>
  <h3>Currency: {{currency}}</h3>
  <h3>States:</h3>
  <input type="text" v-model="searchStateValue" ><button @click="searchState()">Search State Code</button>
  {{searchStateResult.name}} {{searchStateResult.state_code}}
  <ul>
    <li v-for="(state,index) in states" :key="index">{{state.name}}</li>
  </ul>
</div></template>
<script>
export default{
    name: 'QuantumPage',
    components: {

    },
    data() {
        return{
            countries: '',
            country: {
                name: "Select Country"
            },
            currencies: '',
            currency: '',
            stateData: '',
            states: '',
            searchStateValue: '',
            searchStateResult: ''
        }
    },
    methods:{
        async getCountries() {
            let response = await fetch('https://countriesnow.space/api/v0.1/countries/iso')
            // console.log(response);
            let responseData = await response.json();
            this.countries = responseData.data;
            // console.log(responseData.data);
        },
        async getCurrencies() {
            let response = await fetch('https://countriesnow.space/api/v0.1/countries/currency')
            // console.log(response);
            let responseData = await response.json();
            this.currencies = responseData.data;

            // this.countries = responseData.data;
            // console.log(responseData.data);
        },
        async getAllStates() {
            let response = await fetch('https://countriesnow.space/api/v0.1/countries/states')
            // console.log(response);
            let responseData = await response.json();
            // this.currencies = responseData.data;

            // this.countries = responseData.data;
            console.log(responseData.data);
            this.stateData = responseData.data;
        },
        getCurrency(country){
            this.country = country;
            // console.log(this.currencies);
            this.currencies.forEach(currency => {
                // console.log(currency.name);
                // console.log(this.country);
                if(currency.name == this.country.name) {
                    // console.log(currency.name);
                    this.currency = currency.currency;
                }
            })
        },
        getState() {
            this.stateData.forEach(data => {
                
                if(data.name == this.country.name) {
                    // console.log(data.name);
                    // console.log(currency.name);
                    this.states = data.states;
                    console.log(this.states);
                    // console.log(this.country.name);
                }
            
            })
        },
        searchState() {
            // console.log("in earch state/");
            // console.log(this.states);
            this.states.forEach(state => {
                // console.log(state);
                if(this.searchStateValue.toLowerCase() == state.name.toLowerCase()){
                    this.searchStateResult = state
                    console.log(this.searchStateResult);
                }
            })
        }
    },
    mounted() {
        this.getCountries();
        this.getCurrencies();
        this.getAllStates();
    }
}
</script>

    