<template>
    <div class="calcultor">
        <div class="calculator-screen fixed right-setting bottom-5 rounded bg-green-200 p-7">
            <div class="text-left">
            <p class="font-bold">Calories: {{calcData.calories}}</p>
                <div class="flex mt-1">
                    <p class="mr-1 ">P:{{calcData.protein}}</p>
                    <p class="mr-1">F:{{calcData.fat}}</p>
                    <p class="mr-1">C:{{calcData.carbohydrate}}</p>
                </div>
                
                <div class="flex">
                    <p v-for="(food, index) in calcData.foods" v-bind:key="index" class="mr-1">
                        {{food}}
                    </p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "Calculator",
    props: {
        calcData: Object,
        foods: Array,
    },
}
</script>

<style scoped>
.right-setting {
    right: 6%;
}

.calculator-screen {
    width: 230px;
}

@media screen and (min-width:1028px) {
    .right-setting {
        right: 16.5%;
    }
}
</style>