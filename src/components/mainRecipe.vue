<script setup>
import { ref, inject } from 'vue'
import axios from 'axios'
import { pageNumber, query } from './globalVar'

let serch = ref('')

const props = defineProps({
  recipe: Array
})

function serchRecipe() {
  query.value = serch.value
}

function addLike(recipe) {
  if (recipe.isLiked) {
    recipe.isLiked = false
    event.target.classList.remove('active')
    axios.delete(`https://6d2a810cc03d9116.mokky.dev/like/${recipe.serverId}`)
  } else {
    recipe.isLiked = true
    event.target.classList.add('active')
    axios
      .post('https://6d2a810cc03d9116.mokky.dev/like', recipe)
      .then((response) => {
        recipe.serverId = response.data.id
      })
      .catch((error) => {
        console.error(error)
      })
  }
}

let empty = inject('empty')
</script>

<template>
  <div class="main__recipe__container">
    <div class="main__recipe__serch">
      <form @submit.prevent="serchRecipe" action="/">
        <input v-model="serch" type="text" name="serch" placeholder="Введите название..." />
        <button type="submit">найти</button>
      </form>
    </div>
    <div class="main__recipe__wrapp">
      <div class="recipe" v-for="(recipe, index) in props.recipe" :key="index">
        <v-expansion-panels>
          <v-expansion-panel>
            <v-expansion-panel-title class="recipe__title">{{
              recipe.title
            }}</v-expansion-panel-title>
            <v-expansion-panel-text class="recipe__text">
              <ul>
                <li><strong>Ингридиенты: </strong>{{ recipe.ingredients }}</li>
                <li><strong>Рецепт: </strong>{{ recipe.instructions }}</li>
              </ul>
            </v-expansion-panel-text>
          </v-expansion-panel>
        </v-expansion-panels>
        <img
          :class="{ active: recipe.isLiked }"
          class="like"
          src="/src/assets/image/nolike.png"
          alt=""
          @click="addLike(recipe, index)"
        />
      </div>
    </div>
    <v-pagination v-model="pageNumber" :length="5"></v-pagination>
    <img v-show="empty" class="empty loader" src="/src/assets/image/empty.png" alt="" />
  </div>
</template>

<style scoped>
.main__recipe__container {
  width: 70%;
  padding: 50px 60px;
  display: flex;
  flex-direction: column;
  row-gap: 90px;
}
.main__recipe__serch {
  display: flex;
  justify-content: center;
}
.main__recipe__wrapp {
  display: flex;
  flex-direction: column;
  row-gap: 40px;
  min-height: 50%;
}
.like {
  width: 40px;
  padding: 5px;
  cursor: pointer;
}
.like:hover {
  background-color: rgba(0, 0, 0, 0.089);
  border-radius: 10px;
}
form {
  display: flex;
  column-gap: 5px;
}
form input {
  border: none;
  background-color: rgba(0, 0, 0, 0.089);
  padding: 7px 20px;
  border-radius: 10px;
  width: 300px;
}
form button {
  border-radius: 10px;
  background: rgb(196, 196, 196);
  padding: 7px 10px;
}
.recipe__title {
  background-color: rgba(0, 0, 0, 0.068);
  font-size: 20px;
}
.recipe__text ul {
  padding: 0 15px;
  display: flex;
  flex-direction: column;
  row-gap: 15px;
  font-size: 20px;
}
.recipe__text {
  border-top: 1px rgba(0, 0, 0, 0.103) solid;
}
.recipe__title {
  display: flex;
  justify-content: space-between;
  gap: 50px;
}
.recipe {
  display: flex;
  align-items: start;
  column-gap: 15px;
}
.active {
  background-color: rgba(0, 0, 0, 0.13);
  border-radius: 10px;
}
.empty {
  position: absolute;
  left: 55%;
  top: 40%;
  z-index: 10;
}
@media (max-width: 750px) {
  form input {
    width: 160px;
    font-size: 12px;
  }
  form button {
    font-size: 10px;
    padding: 7px 5px;
  }
  .recipe .recipe__title{
    font-size: 15px;
  }
  .like{
    width: 30px;
  }
  .recipe__text ul li{
    font-size: 14px;
  }
}
@media (max-width: 570px) {
  .main__recipe__container {
    width: 100%;
  }
}
</style>
