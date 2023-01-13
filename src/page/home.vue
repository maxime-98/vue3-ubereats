<template>
  <div class="home">
    <RestaurantRow v-for="(data,i) in data_restaurant" :key="i" :three_restaurant="data"/>
  </div>
</template>

<script>
//IMPORT
import BDD from '../BDD.js'
import { onMounted, ref } from 'vue'

//COMPONENTS
import RestaurantRow from '../components/RestaurantRow.vue'

export default {
    name : 'VHome',
    components : {
        RestaurantRow,
    },
    //setup est le block qui contient tout le code js qui est écrit dans une page Vue
    setup() {
        //On crée une classe qui represente le modèle de donnée qu'on v arecevoir, soirt un JSON ou sous une autre forme
        class Restaurant {
            constructor(name, note, image, drive_time) {
                this.name = name
                this.note = note
                this.image = image
                this.drive_time = drive_time
            }
        }

        //On crée un tableau qui va contenir les différentes données de chaque restaurant
        let data_restaurant = ref([]);

        //On écrit une méthode qui va créer, remplir et désemplir un tableau(three_restaurant) de 3 données de reataurant
        //Lorsque le 2e tableau(three_restaurant) est à 2 éléments, il ajoute un troisième élément, puis va remplir le grand tableau
        //puis remplis la prémiere case de dataRestaurant puis se vide et recommence
        /* const makeDataRestaurant = () => {
            //
        }
        équivaut à
        const makeDataRestaurant() {
            //
        }
        */
        const makeDataRestaurant = () => {
            let three_restaurant = [];

            for(const restaurant of BDD) {
                const new_restaurant = new Restaurant(restaurant.name, restaurant.note, restaurant.image, restaurant.drive_time)

                if(three_restaurant.length === 2) {
                    three_restaurant.push(new_restaurant);
                    data_restaurant.value.push(three_restaurant);
                    three_restaurant = [];
                } else {
                    three_restaurant.push(new_restaurant);
                }
            }
            console.log(data_restaurant);
        }

        //Les bonnes pratique exigents que la fonction soit appelé lors du cycle onMounted
        //il existe plusieurs cycles de vie d'une apllication, les plus connues sont Created, onMounted, Destroyed
        onMounted(makeDataRestaurant);
        
        //return
        return {
            data_restaurant,
        }
    },
}
</script>

<style>

</style>