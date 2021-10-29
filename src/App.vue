<template>
  <img alt="Vue logo" src="./assets/logo.png" width="100">
  <h1>Nutrition Calculator</h1>
  <button v-on:click="getData">Get Data</button>
  <div class="nutrition">
    <p>food: {{ food_name }}</p>
    <p>calories: {{ calories }}</p>
    <p>carbohydrate: {{ carbohydrate }}</p>
    <p>fat: {{ fat }}</p>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  data () {
    return {
      food_name: "",
      calories: 0,
      carbohydrate: 0,
      protein: 0,
      fat: 0,
    }
  },
  methods: {
    getData: function() {
    const apiEndpoint = `https://trackapi.nutritionix.com/v2/natural/nutrients`;
    const params = { query: "beer" };
    const x_app_id = process.env.VUE_APP_X_APP_ID;
    const x_app_key = process.env.VUE_APP_X_APP_KEY;
    axios.post(apiEndpoint, params, {
      headers: {
        "x-app-id" : x_app_id,
        "x-app-key": x_app_key
      }
    }).then((res) => {
      console.log(res.data.foods[0].food_name);
      console.log(res.data.foods[0].nf_calories);
      console.log(res.data.foods[0].nf_total_carbohydrate);
      console.log(res.data.foods[0].nf_protein);
      console.log(res.data.foods[0].nf_total_fat);


      this.food_name = res.data.foods[0].food_name
      this.calories = res.data.foods[0].nf_calories
      this.carbohydrate = res.data.foods[0].nf_total_carbohydrate
      this.protein = res.data.foods[0].nf_protein
      this.fat = res.data.foods[0].nf_total_fat
    });
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
