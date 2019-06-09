<template>
  <div class="home">
    <h1>PLANT INFO</h1>
    <v-layout row wrap class="card" v-for="plant in records" :key="plant.id">
      <v-flex md7>
        <v-card class="pa-3 ma-2">
          <h2 class="mb-1 ml-2">{{ plant.fields.CommonName1 }}</h2>
          <v-card-text>
            <h3 v-if="plant.fields.CommonName2 != null">
              Other Names: {{ plant.fields.CommonName2 }}
              <span
                v-if="plant.fields.CommonName3 != null"
              >or {{ plant.fields.CommonName3 }}</span>
            </h3>
            <p class="scientific-name pb-1">Scientific Name: {{ plant.fields.ScientificName }}</p>
          </v-card-text>
          <v-img src="https://loremflickr.com/320/320/plant"></v-img>
        </v-card>
      </v-flex>

      <v-flex md5>
        <v-card class="information pa-3 ma-2">
          <h3 class="mb-3">Information</h3>
          <v-card-text class="light">
            <p>
              <span class="bold">Light:</span>
              <v-chip>{{ plant.fields.Light.join(' to ') }}</v-chip>
            </p>
            <p class="italic">This is a light description</p>
          </v-card-text>
          <v-card-text class="water">
            <p v-if="plant.fields.Water.includes('Misting')">
              <span class="bold">Water:</span>
              <v-chip>{{ plant.fields.Water.slice(0, -1).join(' to ') + ' and ' + plant.fields.Water.slice(-1) }}</v-chip>
            </p>
            <p v-else>
              <span class="bold">Water:</span>
              <v-chip>{{ plant.fields.Water.join(' to ') }}</v-chip>
            </p>
            <p class="italic">Water once a week.</p>
          </v-card-text>
          <v-card-text class="soil">
            <p>
              <span class="bold">Soil:</span>
              <v-chip>{{ plant.fields.Soil.join(', ') }}</v-chip>
            </p>
            <p class="italic">blah blah blah</p>
          </v-card-text>
          <v-card-text class="category">
            <p>
              <span class="bold">Category:</span>
              <v-chip>{{ plant.fields.Category.join(', ') }}</v-chip>
            </p>
            <p class="italic">blah blah blah</p>
          </v-card-text>
          <v-card-text class="native-to">
            <CountriesMap/>
            <br>
            <p>
              <span class="bold">Native to:</span>
              <v-chip>United States of America</v-chip>
            </p>
          </v-card-text>
        </v-card>
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
  margin-bottom: 1rem;
  .scientific-name {
    font-style: italic;
  }
  p {
    margin-bottom: 0px;
  }
  h2 {
    font-family: "Playfair Display", serif;
    font-size: 2.5rem;
    border-bottom: 2px solid black;
    display: inline-block;
  }
  .information {
    h3 {
      font-size: 1.5rem;
      border-bottom: 2px solid black;
    }
    .bold {
      background: #242424;
      color: white;
      padding: 5px 35px 8px 20px;
      margin-left: -5px;
      margin-right: -32px;
      border-radius: 30px;
    }
    .bold,
    .v-chip {
      border: 1px solid #242424;
    }
  }
  div.map {
    // see components/CountriesMap.vue
    margin: 0 auto;
    max-width: 100%;
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
      fill: orange !important;
    }
  }
}
/* JUST PLACEHOLDERS */
.light .v-chip {
  background: #fff176 !important;
}
.water .v-chip {
  background: #80deea !important;
}
.soil .v-chip {
  background: #b9f6ca !important;
}
.category .v-chip {
  background: #ffab91 !important;
}
</style>
