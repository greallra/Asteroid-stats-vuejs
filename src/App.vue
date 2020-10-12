<template>
<div id="App">
  <!-- header -->
  <header class=" has-background-danger-light">
    <h1 class="has-text-centered is-size-4 has-text-weight-bold has-text-warning pt-2">Ultimate Asteroid Stats</h1>
  </header>
  <!-- Nav Tabs -->
  <div class="tabs is-centered has-background-danger-light">
    <ul>
      <li  :class="activePage === 'stats' ? 'is-active': '' " @click="activate('stats')"><a :class="activePage === 'stats' ? 'has-text-warning has-text-weight-semibold': 'has-text-white' ">Stats</a></li>
      <li  :class="activePage === 'randomAsteroids' ? 'is-active': '' " @click="activate('randomAsteroids')"><a :class="activePage === 'randomAsteroids' ? 'has-text-warning has-text-weight-semibold': 'has-text-white' ">Random Asteroids</a></li>
      <li  :class="activePage === 'datePicker' ? 'is-active': '' " @click="activate('datePicker')"><a :class="activePage === 'datePicker' ? 'has-text-warning has-text-weight-semibold': 'has-text-white' ">Between 2 Date ranges</a></li>
      <li  :class="activePage === 'searchById' ? 'is-active': '' " @click="activate('searchById')"><a :class="activePage === 'searchById' ? 'has-text-warning has-text-weight-semibold': 'has-text-white' ">Search With id</a></li>
    </ul>
  </div>

  <!-- Pages -->
  <Stats :activePage="activePage"/>
  <RandomAsteroids  :activePage="activePage"/>
  <DateSearch :activePage="activePage"></DateSearch>
  <SearchById :activePage="activePage"/>
 
</div>
</template>

<script>
import RandomAsteroids from './components/RandomAsteroids.vue';
import DateSearch from './components/DateSearch.vue';
import SearchById from './components/SearchById.vue';
import Stats from './components/Stats';

export default {
  name: 'App',
  components: {
    RandomAsteroids,
    DateSearch,
    SearchById,
    Stats,
    
  },
  data(){
    return {
      //controls what page is rendered
      activePage: 'stats',
    }
  },
  methods: {
    //controls what page is rendered
    activate(page){
      this.activePage = page
      console.log(page);
    }
  }
}
</script>

<style>
#App {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  overflow: hidden;

}
header {
  background: radial-gradient(circle at bottom, navy 0, black 100%);
   height: 60px;
}
.tabs {
  background: radial-gradient(circle at bottom, navy 0, black 100%);
}

[type="date"] {
  background:#fff url(https://cdn1.iconfinder.com/data/icons/cc_mono_icon_set/blacks/16x16/calendar_2.png)  97% 50% no-repeat ;
}
[type="date"]::-webkit-inner-spin-button {
  display: none;
}
[type="date"]::-webkit-calendar-picker-indicator {
  opacity: 0;
}

/* custom styles */

label {
  padding: 10px;
}
input {
  border: 1px solid #c4c4c4;
  border-radius: 5px;
  background-color: #fff;
  padding: 3px 5px;
  box-shadow: inset 0 3px 6px rgba(0,0,0,0.1);
  width: 190px;
}
</style>
