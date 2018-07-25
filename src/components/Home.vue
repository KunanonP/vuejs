<template>
  <div class="Home">
    Search nasa picture
    <div class="container">
      <form v-on:submit.pervent="getSearch(query)">
        <input type="text" placeholder="Search" v-model="query"/>
      </form>
      <div v-if="results">
        <div class="example2" v-for="result in results">
          <div>
            <img class="example" v-bind:src="result.links[0].href" />
          </div>
          <div>
            <h3 v-bind:value="result.data[0].nasa_id">
              {{result.data[0].title}}
            </h3>
            <p>
              {{result.data[0].description | subStr}}
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default{
  name:"Home",

  data(){
    return {
      query: '',
      results: '',
    }
  },

  methods: {
    getSearch(query){
      axios.get('https://images-api.nasa.gov/search?q='+query+'&media_type=image')
      .then(response =>{
        console.log(response.data.collection.items);
        this.results = response.data.collection.items
      })
    }
  },

  filters: {
    subStr: function(string) {
      return string.substring(0,150) + '...';
    }
  }
}
</script>

<style scoped>
  .example {
    width: 256px;
    padding: 10px;
  }

  .example2 {
    border-top: 1px solid lightgrey;
    padding-top: 20px;
  }
</style>
