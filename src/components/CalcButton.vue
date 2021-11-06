<template>
    <div>
        <button v-on:click="calculate($parent.calcData)" class="mt-1 px-3 py-2 flex items-center justify-center rounded-full bg-yellow-500 hover:bg-yellow-600 text-white" type="submit">
            +
        </button>
    </div>
</template>

<script>
export default {
    name: "CalcButton",
    props: {
        food: Object,
    },
    methods: {
        calculate: function(data) {
            data.calories += Math.round(this.food.nf_calories)
            data.protein += Math.round(this.food.nf_protein)
            data.fat += Math.round(this.food.nf_total_fat)
            data.carbohydrate += Math.round(this.food.nf_total_carbohydrate)
            data.foods.push(this.food.food_name)
            this.$parent.calcData = data
            
        }
    }
}
</script>

<style scoped>

</style>