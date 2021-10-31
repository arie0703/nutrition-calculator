<template>
  <img alt="Vue logo" src="./assets/logo.png" width="100">
  <h1>Nutrition Calculator</h1>
  <input type="text" v-model="search_param">
  <button v-on:click="getData">Get Data</button>
  <div class="foods">
    <ion-card v-for="(food, index) in foods" v-bind:key="index">
      <div class="nutrition-info">
      <h2>{{ food.food_name }}</h2>
      <li>calories: {{ food.nf_calories }}</li>
      <li>protein: {{ food.nf_protein }}</li>
      <li>carbohydrate: {{ food.nf_total_carbohydrate }}</li>
      <li>fat: {{ food.nf_total_fat }}</li>
      </div>

      <div class="food-pic">
        <img :src="food.photo.thumb">
      </div>
    </ion-card>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  data () {
    return {
      search_param: "beer",
      foods: [],
    }
  },
  methods: {
    getData: function() {
      const apiEndpoint = `https://trackapi.nutritionix.com/v2/natural/nutrients`;
      const params = { query: this.search_param };
      const x_app_id = process.env.VUE_APP_X_APP_ID;
      const x_app_key = process.env.VUE_APP_X_APP_KEY;
      axios.post(apiEndpoint, params, {
        headers: {
          "x-app-id" : x_app_id,
          "x-app-key": x_app_key
        }
      }).then((res) => {
        this.foods = res.data.foods
        console.log(this.foods)
      }).catch(() => {
        console.log("Not Found")
      });
    },
  }
}
</script>

<style scoped>

ion-card {
  margin: 0 auto;
  width: 400px;
  padding: 10px;
  text-align: left;
  display: flex;
  margin-top: 10px;
  color: #222;
}

.food-pic {
  width: 40%;
  margin-left: auto;
  text-align: center;
}

</style>

<style>

:root {
  --ion-background-color: #fff;
  --ion-text-color: #222;
}
body {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  background-color: #ddd;
}
</style>
