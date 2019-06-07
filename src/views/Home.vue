<template>
  <div class="home">
    <h1>Plants!</h1>
    <div class="card" v-for="plant in records" :key="plant.id">
      <h2>{{ plant.fields.CommonName1 }}</h2>
        <div v-if="plant.fields.CommonName2 != null">
          <h3>Other Names: {{ plant.fields.CommonName2 }} <span v-if="plant.fields.CommonName3 != null">or {{ plant.fields.CommonName3 }}</span></h3>
          </div>
        

      <p class="scientific-name">Scientific Name: {{ plant.fields.ScientificName }}</p>
      <p>Light: {{ plant.fields.Light.join(' to ') }}</p>
  
      <p v-if="plant.fields.Water.includes('Misting')">MISTING Water: {{ plant.fields.Water.slice(0, -1).join(' to ') + ' and ' + plant.fields.Water.slice(-1) }}</p>
      <p v-else>Water: {{ plant.fields.Water.join(' to ') }}</p>

      <p>Soil: {{ plant.fields.Soil.join(', ') }}</p>
      <p>Category: {{ plant.fields.Category.join(', ') }}</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: 'home',
  data() {
    return {
    records: ""
    }
  },
  created() {
    this.getPlantInfo();
  },
  methods: {
    getPlantInfo() {
      axios({
        url: process.env.VUE_APP_API_URL + process.env.VUE_APP_API_SUFFIX,
        headers: {
          Authorization: "Bearer " + process.env.VUE_APP_API_KEY
        },
        params: {
          sortField: "ScientificName",
          sortDirection: "asc",
        }
      })
        .then(response => {
          this.records = response.data.records;
          // eslint-disable-next-line
          console.log(response.data.records);
        })
        .catch(error => {
          // eslint-disable-next-line
          console.log("ERROR: " + error);
        });
    }
  }
}
</script>

<style lang="scss" scoped>
.home {
  max-width: 800px;
  margin: 0 auto;
}
.card {
  border: 1px solid black;
  margin-bottom: 1rem; 
  .scientific-name {
    font-style: italic;
  }
}
</style>
