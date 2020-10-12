 
<template>
  <div class="container is-max-desktop" :class="activePage === 'searchById' ? 'is-active': 'is-hidden' ">
    <div class="section">
       <div class="columns is-centered">
          <!-- Id input column -->
          <div class="column is-four-fifths">
            <h3>Enter the Asteroid ID</h3>

            <div class="field">
              <div class="control">
                <input
                  :class="getIputClass"
                  type="number"
                  :value="asteroidId"
                  @input="updateValue"
                />
              </div>
            </div>
            <!-- Helper Message -->
            <div v-if="asteroidId.length > 0 && asteroidId.length < 7">
              {{ helperMsg }}
            </div>
            <div v-if="asteroidId.length === 7">
                <button :class="btn" @click="search">Search</button>
            </div>
            <!-- Error Message -->
            <article class="message is-danger mt-3" v-show="error">
              <div class="message-header">
                <p>Error</p>
              </div>
              <div class="message-body">
                <p>Sorry, couldnt get that, try again</p> 
                <button class="button" @click="resetSearch">Try Again?</button>
              </div>
            </article>
           <!-- Card/result -->
            <div class="card"  v-if="asteroidData">
                <div class="card-content">
                  <div class="media">
                    <div class="media-content">
                      <div class="columns is-0">
                        <div class="column is-full has-text-centered">
                          <img :src="require(`@/assets/asteroid.png`)" alt="Pasteroid">
                          <span class="is-size-6 has-text-weight-bold">Asteroid Name:</span> <span class="is-size-6 has-text-weight-bold has-text-danger">{{ asteroidData.name }}</span>
                        </div>
                      </div>
                      <div class="columns is-0 has-text-centered">
                        <div class="column">
                            <span class="is-4"> <span class="is-size-6 has-text-weight-light">Is hazardous?:</span> <span class="is-size-6 has-text-weight-bold has-text-warning">{{ asteroidData.is_potentially_hazardous_asteroid }}</span></span>
                        </div>
                        <div class="column">
                            <span class="is-4"> <span class="is-size-6 has-text-weight-light ">Miss Distance(Km):</span> <span class="is-size-6 has-text-weight-bold has-text-warning">{{  asteroidData.close_approach_data.length > 0 ? Math.round(asteroidData.close_approach_data[0].miss_distance.kilometers) : 'NA' }}</span></span>
                        </div>
                        <div class="column">
                            <span class="is-4"> <span class="is-size-6 has-text-weight-light ">Diameter(Km):</span> <span class="is-size-6 has-text-weight-bold has-text-warning">{{ Math.round(asteroidData.estimated_diameter.kilometers.estimated_diameter_min)}} - {{Math.round(asteroidData.estimated_diameter.kilometers.estimated_diameter_max) }}</span></span>
                        </div>
                      </div>
                      <div class="columns is-0">
                        <div class="column is-full has-text-centered">
                          <a class="button" :href="asteroidData.nasa_jpl_url" target="#">Read More</a>
                        </div>
                      </div>
                      
                    </div>
                  </div>
                </div>   
              </div> 
              <!-- Reset -->
              <div v-if="asteroidData">
                <button class="button ml-auto mt-3" @click="resetSearch">Reset</button>
              </div>
              
        </div>
     </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
 //let id ="2021277";
  export default {
    name: "SearchById",
 
    props: ['activePage'],
    data() {
      return {
        asteroidData: null,
        asteroidId: '',
        helperMsg: '',
        loading: false,
        error: false
      };
    },
    methods: {
        //update ID on user input and display a helper message
        updateValue(event) {
        const value = event.target.value
        if (String(value).length <= 7) {
          this.asteroidId = value
          this.helperMsg = `Id must be 7 characters long. (You entered ${7 - this.asteroidId.length})`
        }
        this.$forceUpdate()
      },
        //api call
        async search(){
          this.loading = true;
          this.error = false;
          const url = `https://api.nasa.gov/neo/rest/v1/neo/${this.asteroidId}?api_key=${process.env.NODE_ENV_API_KEY}`;
          try {
          let res =  await axios.get(url)
            if(res.status === 200){
              this.asteroidData = res.data;
            }
            if(res.status > 399){
              this.error = true;
            }    
          } catch (error) {
            this.error = true;
              console.log(error);
          }
          this.loading = false;
          
        },
        resetSearch() {
          this.error = false;
          this.asteroidId = '';
          this.asteroidData = null;
        }
    },
    computed: {
      //dynamic classes
      getIputClass(){
        if(this.asteroidId.length === 0){
          return 'input is-info';
        }
        if(this.asteroidId.length > 0 && this.asteroidId.length < 7){
          return 'input is-danger';
        }
        else {
          return 'input is-success'
        }   
      },
      btn() {
        return this.loading ? 'button is-loading' : 'button'
      }
    }
  };
</script> 

<style  scoped>
/* Hide arrows on input */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
    /* display: none; <- Crashes Chrome on hover */
    -webkit-appearance: none;
    margin: 0; /* <-- Apparently some margin are still there even though it's hidden */
}
input[type=number] {
    -moz-appearance:textfield; /* Firefox */
}
</style>
