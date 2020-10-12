<template>
<div>
    <div class="card">
    <div class="card-content" v-for="(obj,i) in data" v-bind:key="i">
      <div class="media" >
        <div class="media-content">
          <div class="columns is-0">
            <div class="column is-full has-text-centered">
               <img :src="require(`@/assets/asteroid.png`)" alt="Placeholder image">
              <span class="is-size-6 has-text-weight-bold">Asteroid Name:</span> <span class="is-size-6 has-text-weight-bold has-text-danger">{{ obj.name }}</span>
            </div>
          </div>
          <div class="columns is-0 has-text-centered">
            <div class="column">
                <span class="is-4"> <span class="is-size-6 has-text-weight-light">Close approach date:</span> <span class="is-size-6 has-text-weight-bold has-text-warning">{{ obj.close_approach_data.length > 0 ? obj.close_approach_data[0].close_approach_date : 'NA'}}</span></span>
            </div>
            <div class="column">
                <span class="is-4"> <span class="is-size-6 has-text-weight-light">Is hazardous?:</span> <span class="is-size-6 has-text-weight-bold has-text-warning">{{ obj.is_potentially_hazardous_asteroid }}</span></span>
            </div>
            <div class="column">
                <span class="is-4"> <span class="is-size-6 has-text-weight-light ">Miss Distance(Km):</span> <span class="is-size-6 has-text-weight-bold has-text-warning">{{  obj.close_approach_data.length > 0 ? Math.round(obj.close_approach_data[0].miss_distance.kilometers) : 'NA' }}</span></span>
            </div>
            <div class="column">
                <span class="is-4"> <span class="is-size-6 has-text-weight-light ">Diameter(Km):</span> <span class="is-size-6 has-text-weight-bold has-text-warning">{{ Math.round(obj.estimated_diameter.kilometers.estimated_diameter_min)}} - {{Math.round(obj.estimated_diameter.kilometers.estimated_diameter_max) }}</span></span>
            </div>
          </div>
          <div class="columns is-0">
            <div class="column is-full has-text-centered">
              <a class="button" :href="obj.nasa_jpl_url" target="#">Read More</a>
            </div>
          </div>
          
        </div>
      </div>
    </div>
   
  </div>
</div>
</template>

<script>
// Dumb resueable component

export default {
  name: 'Card',
  props: ['data'],
}

</script>

<style scoped>
.card-content {
  border-bottom: 1px solid #ddd;
  /* margin: 20px; */
}
</style>