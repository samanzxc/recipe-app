<script setup>
import axios from 'axios'
import { ref, onMounted } from 'vue'

const likeRecipe = ref([])
async function getLikeRecipe(){
  const {data} = await axios.get('https://6d2a810cc03d9116.mokky.dev/like')
  data.forEach(el => {
    likeRecipe.value.push(el)
  });
}
onMounted(getLikeRecipe)


function removeLike(recipeLike){
  recipeLike.isLiked = false;
    axios.delete(`https://6d2a810cc03d9116.mokky.dev/like/${recipeLike.id}`);
    likeRecipe.value = likeRecipe.value.filter((el)=> el.isLiked)
  }

</script>

<template>
  <div class="like__container">
    <p class="title">Понравившиеся рецепты: {{ likeRecipe.length }}</p>
    <div class="recipe__like__container">

      <div class="recipe" v-for="(recipeLike, index) in likeRecipe" :key="index">
        <v-expansion-panels>
          <v-expansion-panel>
            <v-expansion-panel-title class="recipe__title">{{recipeLike.title}}</v-expansion-panel-title>
            <v-expansion-panel-text class="recipe__text">
              <ul>
                <li><strong>Ингридиенты: </strong>{{ recipeLike.ingredients }}</li>
                <li><strong>Рецепт: </strong>{{ recipeLike.instructions }}</li>
              </ul>
            </v-expansion-panel-text>
          </v-expansion-panel>
        </v-expansion-panels>
        <img  @click="removeLike(recipeLike, index)" class="like active" src="/src/assets/image/nolike.png" alt="" />
      </div>
    </div>
    <img v-show="!likeRecipe.length" class="empty" src="/src/assets/image/empty.png" alt="empty">
  </div>
</template>

<style scoped>
.like__container {
  width: 70%;
  padding: 70px;
}
.title {
  font-size: 26px;
  margin-bottom: 50px;
}
.recipe__like__container {
  display: flex;
  flex-direction: column;
  row-gap: 40px;
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
.like {
  width: 40px;
  padding: 5px;
  cursor: pointer;
}
.like:hover {
  background-color: rgba(0, 0, 0, 0.089);
  border-radius: 10px;
}
.active {
  background-color: rgba(0, 0, 0, 0.13);
  border-radius: 10px;
}
.empty {
    width: 100px;
    position: absolute;
    top: 40%;
    left: 65em;
}
@media(max-width: 950px){
  .title{
    font-size: 20px;
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
  .like__container {
    width: 100%;
  }
}
</style>
