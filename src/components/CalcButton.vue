<template>
    <div>
        <button v-on:click="calculate(calcData)" class="mt-1 px-3 py-2 flex items-center justify-center rounded-full bg-yellow-500 hover:bg-yellow-600 text-white" type="submit">
            +
        </button>
    </div>
</template>

<script>
export default {
    name: "CalcButton",
    props: {
        food_name: String,
        calories: Number,
        protein: Number,
        fat: Number,
        carbohydrate: Number,
        calcData: Object,
    },
    methods: {
        calculate: function(data) {
            console.log(data)
            data.calories += Math.round(this.calories)
            data.protein += Math.round(this.protein)
            data.fat += Math.round(this.fat)
            data.carbohydrate += Math.round(this.carbohydrate)
            data.foods.push(this.food_name)
            this.$parent.calcData = data
        }
    }
}
</script>

<style scoped>

</style>