<script setup>
import { ref, watch } from 'vue'
import myRecipe from './myRecipe.vue'
import likeRecipe from './likeRecipe.vue'
import mainRecipe from './mainRecipe.vue'

let activePage = ref('home')

let activeMenu = ref(false)


const props = defineProps({
  recipe: Array
})
</script>

<template>
  <main>
    <img src="/src/assets/image/menu.png" alt="menu" class="menu" @click="activeMenu = !activeMenu">
    <div :class="{activeMn: activeMenu}" class="left__col">
      <img src="/src/assets/image/close.png" alt="close" class="close" @click="activeMenu = !activeMenu">
      <div class="wrapp">
        <div class="left__col__logo"><p>Recipe App</p></div>
        <div class="left__col__navigation">
          <nav>
            <div :class="{ active: activePage == 'home' }" class="nav" @click="activePage = 'home'">
              <img src="/src/assets/image/home.png" alt="home" /><a href="#">Главная</a>
            </div>
            <div
              :class="{ active: activePage == 'recipe' }"
              class="nav"
              @click="activePage = 'recipe'"
            >
              <img src="/src/assets/image/recipe.png" alt="recipe" /><a href="#">Мои рецепты</a>
            </div>
            <div :class="{ active: activePage == 'like' }" class="nav" @click="activePage = 'like'">
              <img src="/src/assets/image/like.png" alt="like" /><a href="#">Понравившиеся</a>
            </div>
          </nav>
        </div>
      </div>
    </div>

    <mainRecipe :recipe="props.recipe" v-if="activePage == 'home'" />

    <myRecipe v-if="activePage == 'recipe'" />
    <likeRecipe v-if="activePage == 'like'" />
  </main>
</template>

<style scoped>
main {
  display: flex;
  position: relative;
}
.left__col {
  display: flex;
  align-items: center;
  flex-direction: column;
  padding: 50px 10px;
  border-right: 1px rgba(0, 0, 0, 0.144) solid;
  min-height: 100dvh;
  width: 400px;
  background-color: #fff;
  transition: 1s;
  z-index: 10;
}
.left__col__logo {
  font-size: 36px;
  margin-bottom: 65px;
}
.nav {
  display: flex;
  column-gap: 15px;
  transition: 0.3s;
  align-items: center;
  padding: 5px 10px;
}
.nav a {
  color: black;
  font-size: 26px;
}
nav {
  display: flex;
  flex-direction: column;
  row-gap: 30px;
  align-items: start;
}
.nav img {
  width: 30px;
}
.active {
  background-color: rgba(0, 0, 0, 0.082);
  border-radius: 10px;
}
.menu {
  width: 30px;
  height: 30px;
  position: relative;
  top: 50px;
  left: 30px;
}
.close {
  display: none;
  position: relative;
  right: -100px;
  top: -40px;
  width: 30px;
  rotate: 180deg;
  opacity: .6;
  cursor: pointer;
}
.close:hover{
  opacity: 1;

}
@media (max-width: 750px) {
  .left__col__logo {
    font-size: 25px;
  }
  .left__col__navigation nav .nav a {
    font-size: 15px;
  }
  .left__col__navigation nav .nav img {
    width: 17px;
  }
}
@media (max-width: 570px) {
  .left__col {
    width: 250px;
    position: absolute;
    left: -700px;
    right: 0;
    top: 0;
    bottom: 0;
  }
  .close{
    display: block;
  }
}
.activeMn {
  left: 0px;
  cursor: pointer;
}

</style>
