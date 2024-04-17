<script setup>
import { ref } from 'vue'
import myRecipe from './myRecipe.vue'
import likeRecipe from './likeRecipe.vue'
import mainRecipe from './mainRecipe.vue'

let activePage = ref('home')

const props = defineProps({
  recipe: Array,

})
</script>

<template>
  <main>
  <div class="left__col">
    <div class="left__col__logo"><p>Recipe App</p></div>
    <div class="left__col__navigation">
      <nav>
        <div :class="{active: activePage == 'home'}" class="nav" @click="activePage = 'home'">
          <img src="/src/assets/image/home.png" alt="home" /><a href="#">Главная</a>
        </div>
        <div :class="{active: activePage == 'recipe'}" class="nav" @click="activePage = 'recipe'">
          <img src="/src/assets/image/recipe.png" alt="recipe" /><a href="#">Мои рецепты</a>
        </div>
        <div :class="{active: activePage == 'like'}" class="nav" @click="activePage = 'like'">
          <img src="/src/assets/image/like.png" alt="like" /><a href="#">Понравившиеся</a>
        </div>
      </nav>
    </div>
  </div>
  
  <mainRecipe 
  :recipe="props.recipe"

   v-if="activePage == 'home'"/>
   
  <myRecipe v-if="activePage == 'recipe'"/>
  <likeRecipe  v-if="activePage == 'like'"/>

</main>
</template>

<style scoped>
main {
  display: flex;
}
.left__col {
  display: flex;
  flex-direction: column;
  padding: 50px 51px;
  border-right: 1px rgba(0, 0, 0, 0.144) solid;
  min-height: 100dvh;
  width: 400px;
}
.left__col__logo {
  font-size: 36px;
  margin-bottom: 65px;
}
.nav {
  display: flex;
  column-gap: 15px;
  transition: .3s;
  align-items: center;
  padding: 5px 10px;
}
.nav a{
  color: black;
  font-size: 26px;
  
}
nav{
  display: flex;
  flex-direction: column;
  row-gap: 30px;
  align-items: start;
  
}
.nav img{
  width: 30px;
}
.active {
  background-color: rgba(0, 0, 0, 0.082);
  border-radius: 10px;
}
</style>
