<template>
    <div class="favorites">
        <p class="py-4">My Favorite</p>
        <div v-for="(favorite, index) in favorites" v-bind:key="index" class="m-auto mt-2 max-w-md py-4 px-8 bg-white shadow-lg rounded-lg">
            <div class="text-left">
                <p class="text-2xl">{{favorite.food_name}}</p>
                <div class="nutrition-info flex">
                    <div class="nutrition-params">
                        <li>calories: {{ favorite.calories }}</li>
                        <li>protein: {{ favorite.protein }}</li>
                        <li>carbohydrate: {{ favorite.carbohydrate }}</li>
                        <li>fat: {{ favorite.fat }}</li>
                    </div>

                    <div class="ml-auto">
                        <img :src="favorite.image_url">
                    </div>
                </div>

                <div class="btn-wrapper flex-auto flex space-x-3">
                  <CalcButton
                    :food_name="favorite.food_name"
                    :calories="favorite.calories"
                    :protein="favorite.protein"
                    :fat="favorite.fat"
                    :carbohydrate="favorite.carbohydrate"
                  />
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import {db} from '../main.js';
import CalcButton from './CalcButton';

export default {
  name: "Favorites",
  props: {
    calcData: Object,
  },
  data () {
    return {
      favorites: [],
    }
  },
  components: {
    CalcButton
  },
  mounted: function() {
    this.fetchFavorites();
  },
  methods: {
      fetchFavorites: function() {
        db.collection("favorites").get()
        .then((querySnapshot) => {
            this.favorites = []
            querySnapshot.forEach((doc) => {
                this.favorites.push(doc.data())
            });
        });
        console.log(this.favorites)
      }
  }  
}
</script>

<style scoped>

</style>