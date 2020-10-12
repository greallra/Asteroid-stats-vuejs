 
<template>
  <div class="container is-max-desktop" :class="activePage === 'datePicker' ? 'is-active': 'is-hidden' ">
    <!-- Filters -->
    <div class="buttons has-addons is-centered">
      <button class="button" :class="sortBy === 'miss-distance' ? 'is-success has-background-danger':''" @click="sort('miss-distance')">Sort By Miss Distance</button>
      <button class="button" :class="sortBy === 'date' ? 'is-success has-background-danger':''" @click="sort('date')">Sort By Date</button>
    </div>
    <!-- Date Picker -->
    <div class="has-text-centered">   
      <label for="from">From</label>
      <input type="date" name="from" id="from" v-model="from">
      <label for="to">To</label>
      <input type="date" name="to" id="to" v-model="to">
    </div>
    <!-- Buttons -->
    <div class="has-text-centered mt-3">   
       <button class="button mr-1 has-background-success-light" @click="makeRequest">Search</button>
       <button v-show="sortedNearEarthObjects.length > 0" class="button ml-1 has-background-danger-light" @click="resetSearch">Reset</button>
    </div>
    <!-- Loading -->
    <div class="has-text-centered mt-3">   
       <PulseLoader :class="loading ? 'is-active': 'is-hidden' "/>   
    </div>
    <!-- Results -->
      <div class="columns is-centered">
        <div class="column is-four-fifths">
           <Card :data="sortedNearEarthObjects"/>
        </div>
      </div>
    <!-- Error -->
    <article class="message is-danger" v-show="error">
      <div class="message-header">
        <p>Error</p>
        <!-- <button class="delete" aria-label="delete"></button> -->
      </div>
      <div class="message-body">
        <p>{{ error }}</p> 
        <button class="button" @click="resetSearch">Try Again?</button>
      </div>
    </article>
  </div>
</template>

<script>
import axios from 'axios';
import PulseLoader from 'vue-spinner/src/PulseLoader.vue'
import Card from './Card';

export default {
  name: "DateSearch",
  props: ['activePage'],

  components: { Card,PulseLoader},
  data() {
    return {
    nearEarthObjects: [],
    sortedNearEarthObjects: [],
    from: '',
    sortBy: '',
    loading: false,
    error: ''
    };
  },
  methods: {
          async makeRequest() {
            this.loading = true;
            this.error = false;
            if(!this.from || !this.to){
              console.log("caught");
               this.loading = false;return;
            }
            try {
               const r = await axios.get(`https://api.nasa.gov/neo/rest/v1/feed?start_date=${this.from}&end_date=${this.to}&detailed=${true}&api_key=${process.env.NODE_ENV_API_KEY}`)
                 console.log("r", r);
                //get all objects onto one array    
                for(var k in r.data.near_earth_objects){
                  r.data.near_earth_objects[k].forEach((o)=>{
                    this.nearEarthObjects.push(o)
                  })
                } 
                this.loading = false;   
            } catch (error) {
              console.log(error);
              this.error = 'Make Sure you choose a period of 7 days or less!'
              this.loading = false;
            }   
            console.log("this.nearEarthObjects", this.nearEarthObjects);
            this.sortedNearEarthObjects = this.nearEarthObjects.slice(0,10);
        },
        resetSearch(){
            this.error = false;
            this.to = '';
            this.from = '';
            this.sortedNearEarthObjects = []
        },
        sort(sortBy){
          this.sortBy = sortBy;
          //copy orginal array
          let copy = [...this.nearEarthObjects]
          this.sortedNearEarthObjects = copy;
          this.loading = true;
           //Sort data by miss distance
           if(sortBy === 'miss-distance'){
             this.sortedNearEarthObjects.sort((a,b)=>{
              if(a.close_approach_data[0].miss_distance.kilometers > b.close_approach_data[0].miss_distance.kilometers) {
                return 1
              }
              else {
                return -1
              }
            }).slice(0,10)
           }
           //Sort data by data
           if(sortBy === 'date'){
             this.sortedNearEarthObjects = this.nearEarthObjects.slice(0,10);
           }
            
          this.loading = false;
        }
  }
    
};
</script> 
