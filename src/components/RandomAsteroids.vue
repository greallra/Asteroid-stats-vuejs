<template>
<div class="container" :class="activePage === 'randomAsteroids' ? 'is-active': 'is-hidden' ">
  <div class="columns is-centered">
    <div class="column is-four-fifths">
      <Card :data="nearEarthObjects"/>
    </div>
  </div>
</div>
</template>

<script>
import axios from 'axios';
import Card from './Card';

export default {
  name: 'TenAsteroids',
  props: ['activePage'],
  components: {
    Card
  },
  data() {
    return {
      nearEarthObjects: []
    }
  },
  created () {
    const url = 'https://api.nasa.gov/neo/rest/v1/neo/browse?page=0&size=10&api_key=${process.env.NODE_ENV_API_KEY}'
    axios.get(url)
    .then((r)=>{
      this.nearEarthObjects = r.data.near_earth_objects.slice(0,10);
      console.log(r);
    })
    .catch((e)=>{
      console.log(e);
    })
 }
}
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.card-content {
  border-bottom: 1px solid #dddd;
}
</style>
