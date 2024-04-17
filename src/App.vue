<script setup>
import { onMounted, ref, watch, provide } from 'vue'
import axios from 'axios'
import leftBar from './components/leftBar.vue';
import {pageNumber, query} from './components/globalVar'

let loader = ref(false)
let empty = ref(false)
//рецепты
let recipe = ref([])
// получение рецептов
async function getRecipe() {
  try {
    loader.value = true
    recipe.value = []
    const {data} =  await axios.get(`https://api.api-ninjas.com/v1/recipe?query=${query.value}&offset=${pageNumber.value}`, {
    headers: {
      'X-Api-Key': 'uBthFZYA+8w2g+6YEiNbuw==kLCINPFVdjoo9LZu'
    }
  })
  data.forEach((item, index) => {
    item.id = index+1; 
    recipe.value.push(item);
  });
  empty.value = false
  } catch (er) {
    console.log(er);
    loader.value = false
    empty.value = true
  }
  finally {
    if (recipe.value.length) {
      empty.value = false
    } else {
      empty.value = true
    }
    loader.value = false
  }
}
onMounted(getRecipe)

watch(pageNumber, getRecipe)
watch(query, getRecipe)
provide('empty', empty)
</script>

<template>
  <leftBar 
  :recipe="recipe"
  :pageNumber="pageNumber"

  />
  <div v-show="loader" class="loader">
    Загрузка...
  </div>
</template>

<style scoped>
.loader{
  position: absolute;
  left: 55%;
  top: 40%;
  z-index: 10;
}
</style>
