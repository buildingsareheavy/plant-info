<template>
  <div class="home">
    <h1>PLANT INFO</h1>
    <v-layout row wrap class="card" v-for="plant in records" :key="plant.id">
      <v-flex md6 align-self-center>
        <h2>{{ plant.fields.CommonName1 }}</h2>
        <h3 v-if="plant.fields.CommonName2 != null">
          Other Names: {{ plant.fields.CommonName2 }}
          <span
            v-if="plant.fields.CommonName3 != null"
          >or {{ plant.fields.CommonName3 }}</span>
        </h3>
        <p class="scientific-name">Scientific Name: {{ plant.fields.ScientificName }}</p>
        <hr>
        <v-carousel cycle="false" height="320">
          <v-carousel-item
            v-for="(photo, index) in photos"
            :key="`photo-${index}`"
            :src="`https://loremflickr.com/320/240/${photo}`"
            reverse-transition="fade"
            transition="fade"
          ></v-carousel-item>
        </v-carousel>
      </v-flex>

      <v-flex md6 align-self-center>
        <CountriesMap/>
        <p>Found in:</p>
        <hr>
        <p>Light: {{ plant.fields.Light.join(' to ') }}</p>
        <hr>
        <p
          v-if="plant.fields.Water.includes('Misting')"
        >MISTING Water: {{ plant.fields.Water.slice(0, -1).join(' to ') + ' and ' + plant.fields.Water.slice(-1) }}</p>
        <p v-else>Water: {{ plant.fields.Water.join(' to ') }}</p>
        <hr>
        <p>Soil: {{ plant.fields.Soil.join(', ') }}</p>
        <hr>
        <p>Category: {{ plant.fields.Category.join(', ') }}</p>
      </v-flex>
    </v-layout>
  </div>
</template>

<script>
import axios from "axios";
import CountriesMap from "@/components/CountriesMap.vue";

export default {
  name: "home",
  components: {
    CountriesMap
  },
  data() {
    return {
      records: "",
      photos: ["plants", "succulents", "houseplants"]
    };
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
          sortDirection: "asc"
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
};
</script>

<style lang="scss">
.card {
  border: 1px solid black;
  margin-bottom: 1rem;
  .scientific-name {
    font-style: italic;
  }
  div.map {
    // see components/CountriesMap.vue
    margin: 0 auto;
    max-width: 50%;
    svg {
      width: 100%;
      height: auto;
      border: 1px solid black;
    }
  }
  svg {
    path {
      fill: #242424;
      stroke: #eeeeee;
      fill-rule: evenodd;
    }
    #US {
      fill: red !important;
    }
  }
}
</style>
