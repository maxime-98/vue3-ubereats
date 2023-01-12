<template>
  <div class="Restaurant--Row">
    <h2 class="title">
        Nos restaurants
    </h2>
    <restaurant-card/>
    </div>
</template>

<script>
import RestaurantCard from './RestaurantCard.vue';

export default {
    name:"RestaurantRow",
    components: {
        RestaurantCard,
    }
}
</script>

<style>

</style>