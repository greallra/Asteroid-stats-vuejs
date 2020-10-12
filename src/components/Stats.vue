<template>
    <div class="container is-max-desktop has-text-centered" :class="activePage === 'stats' ? 'is-active': 'is-hidden' ">
        <p class="is-size-5">Last Update <span class="is-size-">({{stats.last_updated}})</span></p>
        <h2 class="is-size-2">Near Earth Objects in Orbit:</h2>
        <p class="is-size-2 has-text-weight-bold has-text-primary">{{ stats.near_earth_object_count }}</p>
        <h2 class="is-size-2">Close Approach Objects in Orbit:</h2>
        <p class="is-size-2 has-text-weight-bold has-text-link">{{ stats.close_approach_count }}</p>
        
    </div>
</template>
<script>
//extra feature is this stats/home page
import axios from 'axios';

export default {
    props: ['activePage'],
    data(){
        return {
            stats: {
                near_earth_object_count: 0,
                close_approach_count: 0,
                last_updated: "",
                source: "",
                nasa_jpl_url: ""
            }
        }
    },
    //get stats
    async created() {
        try {
            let res = await axios.get('https://api.nasa.gov/neo/rest/v1/stats?api_key=${process.env.NODE_ENV_API_KEY}')
            this.stats = res.data
            
        } catch (error) {
            console.log(error);
        }
    }
}
</script>