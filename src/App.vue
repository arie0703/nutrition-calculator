<template>
  <img alt="Vue logo" src="./assets/logo.png" width="100" class="m-auto">
  <p class="text-3xl mb-2">Nutrition Calculator</p>
  <p>Calories: {{calcData.calories}}</p>
  <p>Protein: {{calcData.protein}}</p>
  <p>Fat: {{calcData.fat}}</p>
  <p>Carbohydrate: {{calcData.carbohydrate}}</p>
  <li v-for="(food, index) in calcData.foods" v-bind:key="index">
    {{food}}
  </li>
  <input type="text" v-model="search_param" class="appearance-none border-2 mr-1 rounded w-200 py-2 px-4 text-gray-700 leading-tight focus:outline-none">
  <button v-on:click="getData" class="bg-yellow-500 hover:bg-yellow-600 text-white py-2 px-4 rounded">Get Data</button>
  <div class="foods">
    <ion-card v-for="(food, index) in foods" v-bind:key="index">
      
        <p class="text-2xl py-1">{{ food.food_name }}</p>
        <div class="nutrition-info flex">
          <div class="nutrition-params">
            <li>calories: {{ food.nf_calories }}</li>
            <li>protein: {{ food.nf_protein }}</li>
            <li>carbohydrate: {{ food.nf_total_carbohydrate }}</li>
            <li>fat: {{ food.nf_total_fat }}</li>
          </div>

          <div class="food-pic">
            <img :src="food.photo.thumb">
          </div>
        </div>

        

        <div class="btn-wrapper flex-auto flex space-x-3">
          <button v-on:click="openModal(food)" class="mt-1 px-1 py-2 flex items-center justify-center rounded-full bg-yellow-500 hover:bg-yellow-600 text-white" type="submit">
            Favorite
          </button>
          <CalcButton
            :food="food"
          />
        </div>
      

      
    </ion-card>

    <Favorites
      ref="favorites"
    />

    <!-- modal -->
    <div v-if="isShowModal" class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity">
      <div class="modal mt-10 bg-white inline-block rounded-lg text-left overflow-hidden shadow-xl transform transition-all sm:my-8 sm:align-middle sm:max-w-lg sm:w-full mt-3 text-center p-5 sm:ml-4 sm:text-left">
        <h3 class="text-lg leading-6 font-medium text-gray-900" id="modal-title">
          Add food to your favorite
        </h3>
        <div class="mt-2">
          <p class="text-sm text-gray-500">
            Are you sure to add this food to your favorite?
          </p>
        </div>
        <div class="py-3 sm:flex">
          <button v-on:click="addFavorite" type="button" class="w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 bg-yellow-600 text-base font-medium text-white hover:bg-yellow-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-yellow-500 sm:ml-3 sm:w-auto sm:text-sm">
            Add
          </button>
          <button v-on:click="isShowModal = false" type="button" class="mt-3 w-full inline-flex justify-center rounded-md border border-gray-300 shadow-sm px-4 py-2 bg-white text-base font-medium text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 sm:mt-0 sm:ml-3 sm:w-auto sm:text-sm">
            Cancel
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import {db} from './main.js';
import Favorites from './components/Favorites';
import CalcButton from './components/CalcButton';

export default {
  name: 'App',
  data () {
    return {
      search_param: "beer",
      foods: [],
      isShowModal: false,
      selected: {},
      calcData: {
        calories: 0,
        protein: 0,
        fat: 0,
        carbohydrate: 0,
        foods: [],
      },
    }
  },
  components: {
    Favorites,
    CalcButton,
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
        console.log(this.favorites)
      }).catch(() => {
        console.log("Not Found")
      });
    },
    openModal: function(food) {
      this.selected = food
      console.log(this.selected)
      this.isShowModal = true
    },
    addFavorite: function() {
      db.collection("favorites").add({
        food_name: this.selected.food_name,
        calories: this.selected.nf_calories,
        protein: this.selected.nf_protein,
        fat: this.selected.nf_total_fat,
        carbohydrate: this.selected.nf_total_carbohydrate,
        image_url: this.selected.photo.thumb,
      })
      .then((res) => {
        console.log(res);
        console.log(this.$refs.favorites)
        this.$refs.favorites.fetchFavorites()
        this.isShowModal = false
      })
      .catch((error) => {
        console.error(error);
      });
    }
  },
}
</script>

<style scoped>

ion-card {
  margin: 0 auto;
  width: 400px;
  padding: 10px;
  text-align: left;
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
