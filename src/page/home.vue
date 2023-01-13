<template>
  <div class="home">
    <div class="header">
        <img src="https://d3i4yxtzktqr9n.cloudfront.net/web-eats-v2/ee037401cb5d31b23cf780808ee4ec1f.svg" alt="">
        <div class="wrapper--input">
            <input v-model="user_search_restaurant" type="text" placeholder="De quoi avez vous envie ?">
            <div class="search">
                <div v-for="(restaurant, i) in search_restaurant" :key="i"  class="container--restaurant--search">
                    <div class="wrapper--img">
                        <img :src="restaurant.image" alt="">
                    </div>

                    <p>{{ restaurant.name }}</p>
                </div>
            </div>
        </div>
    </div>
    <div class="baniere">

    </div>
    <RestaurantRow v-for="(data,i) in data_restaurant" :key="i" :three_restaurant="data"/>
  </div>
</template>

<script>
//IMPORT
import BDD from '../BDD.js'
import { onMounted, ref, watch } from 'vue'

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
        //On va récuperer tout le srestaurnants
        let all_restaurant = [];

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
                //make all restaurant array
                all_restaurant.push(new_restaurant);

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

        //User search restaurant
        let user_search_restaurant = ref('');
        let search_restaurant = ref([]);

        watch(user_search_restaurant,(new_value) => {
            //console.log(new_value, all_restaurant);
            let regex = RegExp(new_value.toLowerCase());

           let new_search_restaurant = all_restaurant.filter(restaurant => regex.test(restaurant.name.toLowerCase()));

           //condition
           /*if(new_value == 0) {
            search_restaurant.value = []
           } else {
            search_restaurant.value = new_search_restaurant;
           }  */

           //condition ternaire
           new_value == 0 ? search_restaurant.value = [] : search_restaurant.value = new_search_restaurant;

        })
        //

        //Les bonnes pratique exigents que la fonction soit appelé lors du cycle onMounted
        //il existe plusieurs cycles de vie d'une apllication, les plus connues sont Created, onMounted, Destroyed
        onMounted(makeDataRestaurant);

        //return
        return {
            data_restaurant,
            user_search_restaurant,
            search_restaurant,
        }
    },
}
</script>

<style lang="scss">
.home{
    .header{
        height: 120px;
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: space-between;
        img {
            width: 200px;
        }
        .wrapper--input {
            position: relative;

            input {
            background-color: #f6f6f6;
            border:none;
            height: 60px;
            width: 400px;
            outline: none;
            padding-left: 20px;
            }
            .search {
                position: absolute;
                top: 100%;
                width: 100%;
                background-color: #fff;

                .container--restaurant--search {
                    display: flex;
                    align-items: center;
                    padding: 10px;

                    .wrapper--img {
                        height: 60px;
                        width: 60px;
                        margin-right: 25px;
                        border-radius: 50%;
                        overflow: hidden;

                        img {
                            height: 60px;
                            width: auto;
                        }
                    }
                }
            }
        }

    }
    .baniere{
        height: 200px;
        width: 100%;
        background-image: url("https://cdn.shopify.com/s/files/1/0619/0274/9931/products/VinylBanner-V1-FastFood_1946x.jpg?v=1649202951");
        background-size: cover;
        background-position: center center;
    }
}
</style>