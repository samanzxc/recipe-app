<script setup>
import { reactive, ref, watch } from 'vue'

let dialog = ref(false)

let data = localStorage.getItem('myRecipeJson')
let myRecipeJson = ref([])

if (data) {
    myRecipeJson.value = JSON.parse(data)
}

let recipeName = ref('')
let ing = ref('')
let myNewRecipe = ref('')

function createRecipe() {
  let myRecipe = reactive({
    recipeName: recipeName.value,
    recipeIng: ing.value,
    newRecipe: myNewRecipe.value
  })

  myRecipeJson.value.push(myRecipe)
  localStorage.setItem('myRecipeJson', JSON.stringify(myRecipeJson.value))
  recipeName.value = ''
  ing.value = ''
  myNewRecipe.value = ''
  dialog.value = false 
}

function deletMyRecipe(index){
    myRecipeJson.value.splice(index, 1)
    localStorage.setItem('myRecipeJson', JSON.stringify(myRecipeJson.value))
}

</script>

<template>
  <div class="myRecipe__container">
    <div @click="dialog = true" class="add__recipe">
      <p>+</p>
      <h4>Создать рецепт</h4>
    </div>
    <div class="main__recipe__wrapp">
      <div class="recipe" v-for="(recipe, index) in myRecipeJson" :key="index">
        <v-expansion-panels>
          <v-expansion-panel>
            <v-expansion-panel-title class="recipe__title">{{ recipe.recipeName }}</v-expansion-panel-title>
            <v-expansion-panel-text class="recipe__text">
              <ul>
                <li><strong>Ингридиенты: </strong>{{ recipe.recipeIng }}</li>
                <li><strong>Рецепт: </strong>{{ recipe.newRecipe }}</li>
              </ul>
            </v-expansion-panel-text>
          </v-expansion-panel>
        </v-expansion-panels>
        <p class="delet" @click="deletMyRecipe(index)">✖</p>
      </div>
    </div>

    <div class="text-center pa-4">
      <v-dialog class="window" v-model="dialog" width="auto">
        <v-card max-width="400">
          <form action="/" @submit.prevent="createRecipe">
            <input v-model="recipeName" class="inp" type="text" placeholder="Введите название" />
            <input v-model="ing" class="inp" type="text" placeholder="Введите ингридиенты" />
            <textarea
              v-model="myNewRecipe"
              class="inp"
              cols="30"
              rows="3"
              placeholder="Введите рецепт"
            ></textarea>
            <button type="submit">Создать</button>
          </form>
          <div class="btn__wrapp">
            <button class="btn__window" @click="dialog = false">закрыть</button>
          </div>
        </v-card>
      </v-dialog>
    </div>
    <img v-show="!myRecipeJson.length" class="empty" src="/src/assets/image/empty.png" alt="empty">
  </div>
</template>

<style scoped>
.myRecipe__container {
  width: 70%;
  padding: 70px;
  display: flex;

  flex-direction: column;
  row-gap: 90px;
}
.add__recipe {
  display: flex;
  column-gap: 10px;
  align-items: center;
  justify-content: center;
  font-size: 25px;
  opacity: 0.6;
  cursor: pointer;
  user-select: none;
}
.add__recipe:hover {
  opacity: 1;
}
.add__recipe p {
  font-size: 50px;
  position: relative;
  top: -3px;
}
.main__recipe__wrapp {
  display: flex;
  flex-direction: column;
  row-gap: 40px;
  min-height: 50%;
}

.recipe__title {
  background-color: rgba(0, 0, 0, 0.068);
  font-size: 20px;
  display: flex;
  justify-content: space-between;
  gap: 50px;
}
.recipe__text ul {
  padding: 0 15px;
  display: flex;
  flex-direction: column;
  row-gap: 15px;
  font-size: 20px;
}
.recipe__text ul li {
    word-break: break-all;
}
.recipe__text {
  border-top: 1px rgba(0, 0, 0, 0.103) solid;
}
.recipe {
  display: flex;
  align-items: start;
  column-gap: 15px;
}
form {
  display: flex;
  flex-direction: column;
  align-items: center;
  row-gap: 15px;
  margin: 25px;
}
.inp {
  border: 1px rgba(0, 0, 0, 0.11) solid;
  font-size: 17px;
  padding: 7px 15px;
  width: 330px;
  border-radius: 7px;
}
.inp:focus {
  outline: 1px rgba(0, 0, 0, 0.466) solid;
}
form button {
  background-color: rgb(238, 238, 238);
  padding: 5px 15px;
  font-size: 19px;
  border-radius: 5px;
}
form button:hover {
  background-color: rgb(219, 219, 219);
}
.btn__wrapp {
  display: flex;
  justify-content: end;
  margin: 15px;
}
.btn__window {
  padding: 5px 10px;
  font-size: 15px;
  border-radius: 5px;
}
.btn__window:hover {
  background-color: #0000000c;
}
.delet{
    font-size: 25px;
    position: relative;
    top: 7px;
    cursor: pointer;
    padding: 0px 7px;
    border-radius: 6px;
    opacity: .7;
}
.delet:hover{
    background-color: #0000000c;
}
.empty {
    width: 100px;
    position: absolute;
    top: 40%;
    left: 65em;
}
@media (max-width: 570px) {
  .myRecipe__container {
    width: 100%;
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
</style>
