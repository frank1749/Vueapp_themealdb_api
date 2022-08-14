<template>
    <h3>Desea agregar una receta?</h3>

   <input type="text" v-model="search" v-on:keyup.enter="searchData" placeholder="Buscar receta" class="search-input" />

   <Meal v-for="meal in meals" v-bind:key="meal.idMeal" v-bind:meal="meal" />

   <div class="text-center">
      ...
   </div>

   <h3>Buscar por categoría</h3>

   <Category v-for="category in paginated" v-bind:key="category.idCategory" v-bind:category="category" />

   <div class="text-center">
      Actual: {{ current }}
   </div>

   <div class="text-center">
      <a @click="prev()"> Anterior </a>
      |
      <a @click="next()"> Siguiente </a>
   </div>
</template>

<script>
   import axios from "axios";
   import Swal from "sweetalert";
   import Category from "./Category.vue";
   import Meal from "./Meal.vue";
   
   export default {
      name: "App",
      components: {
         Category,
         Meal,
      },
      data() {
         return {
            categories: [],
            search: null,
            meals: [],
            //Paginate
            current: 1,
            pageSize: 5,
         };
      },
      mounted() {
         axios
            .get("https://www.themealdb.com/api/json/v1/1/categories.php")
            .then((res) => {
               console.log(res.data.categories);

               this.categories = res.data.categories;
            })
            .catch((err) => {
               console.log(err);
            });
      },

      computed: {
         indexStart() {
            return (this.current - 1) * this.pageSize;
         },
         indexEnd() {
            return this.indexStart + this.pageSize;
         },
         paginated() {
            return this.categories.slice(this.indexStart, this.indexEnd);
         },
      },

      methods: {
         searchData() {
            if (this.search) {
               axios
                  .get("https://www.themealdb.com/api/json/v1/1/search.php?s=" + this.search)
                  .then((res) => {
                     console.log(res.data.meals);
                     this.meals = res.data.meals;

                     console.log("res meals", this.meals);
                  })
                  .catch((err) => {
                     console.log(err);
                  });
            } else {
               // alert('Ingresa texto a buscar!!');
               swal({
                  title: "Atención",
                  text: "Ingresa texto a buscar!",
                  icon: "error",
               });
            }
         },
         prev() {
            this.current--;
         },
         next() {
            this.current++;
         },
      },
   };
</script>

<style scoped>
   .logo {
      height: 6em;
      padding: 1.5em;
      will-change: filter;
   }
   .logo:hover {
      filter: drop-shadow(0 0 2em #646cffaa);
   }
   .logo.vue:hover {
      filter: drop-shadow(0 0 2em #42b883aa);
   }
   .category_container {
      border: 1px solid yellow;
      padding: 50px;
   }
   .search-input {
      border: none;
      border-bottom: 1px solid yellow;
      margin-bottom: 20px;
      min-height: 20px;
      min-width: 20px;
   }

   * {
      font-family: monospace;
   }
</style>