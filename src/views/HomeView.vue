<template>
  <div class="home">
      <h1>My Recipes</h1>
      <button @click="toggleModal">Add New Recipe</button>


      <div class="recipes">

        <div class="card" v-for="recipe in $store.state.recipes" :key="recipe.slug">
          <h2>{{recipe.title}}</h2>
          <p>{{recipe.description}}</p>
          <router-link :to="`/recipe/${recipe.slug}`">
            View More
          </router-link>
        </div>
      </div>


      <div class="recipe-modal" v-if="modalOpen">
          <div class="recipe-modal-content">

              <h2>Add New Recipe Content Here....</h2>
              <form @submit.prevent="addNewRecipe">
                  <div class="form-group">
                      <label for="Name">Name</label>
                      <input type="text" id="Name" v-model="newRecipe.title"/>
                  </div>
                  <div class="form-group">
                      <label for="Descirption">Descirption</label>
                      <textarea id="Descirption" v-model="newRecipe.description"></textarea>
                  </div>
                  <div class="form-group">
                      <label for="Ingredients">Ingredients</label>

                      <div class="ingredients" v-for="i in newRecipe.ingredientCount" :key="i">
                        <input type="text" v-model="newRecipe.ingredients[i-1]"/>
                      </div>

                      <button type="button" @click="addNewIngredients">Add Ingredients</button>
                  </div>
                  <div class="form-group">
                      <label for="Method">Method</label>

                      <div class="method"  v-for="i in newRecipe.methodCount" :key="i">
                          <textarea id="Method" v-model="newRecipe.methods[i-1]"></textarea>
                      </div>

                      <button type="button" @click="addNewMethod">Add Steps</button>
                  </div>

                  <button type="submit"> Save Recipe </button>
                  <button type="button" @click="toggleModal"> Close </button>
              </form>
          </div>
      </div>
  </div>
</template>

<script>

import {ref} from "vue"
import { useStore} from "vuex"
export default {

  name: 'HomeView',
  setup(){

    const newRecipe = ref({
      title: "",
      description: "",
      ingredients: [],
      methods: [],
      ingredientCount: 1,
      methodCount: 1
    })

    const modalOpen = ref(false);
    const store = useStore();

    const toggleModal = () =>{
      modalOpen.value = !modalOpen.value;
    }

    const addNewIngredients = () =>{
      newRecipe.value.ingredientCount++
    }

    const addNewMethod = () =>{
      newRecipe.value.methodCount++
    }

    const addNewRecipe = () => {
        newRecipe.value.slug = newRecipe.value.title.toLowerCase().replace(/\s/g, '-');

        if(newRecipe.value.slug == ""){
            alert("Kindly fill up the title");
            return ;
        }

        store.commit('ADD_RECIPE', {...newRecipe.value});

        newRecipe.value = {
            title: "",
            description: "",
            ingredients: [],
            methods: [],
            ingredientCount: 1,
            methodCount: 1
          }
        toggleModal();
    }

    return {
      newRecipe,
      toggleModal,
      modalOpen,
      addNewIngredients,
      addNewMethod,
      addNewRecipe
    }
  }
}
</script>

<style>
.home {
  padding: 1rem;
  display: flex;
  flex-direction: column;
  align-items: center;
}
h1 {
  font-size: 3rem;
  margin-bottom: 32px;
}
.recipes {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
}
.recipes .card {
  padding: 1rem;
  border-radius: 5px;
  margin: 1rem;
  background-color: #081c33;
}
.recipes .card h2 {
  font-size: 2rem;
  margin-bottom: 1rem;
}
.recipes .card p {
  font-size: 1.125rem;
  line-height: 1.4;
  margin-bottom: 1rem;
}
.recipe-modal {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}
.recipe-modal .recipe-modal-content {
  background-color: #081c33;
  padding: 2rem;
  border-radius: 1rem;
  width: 100%;
  max-width: 768px;
}
.recipe-modal-content h2 {
  font-size: 2rem;
  margin-bottom: 1rem;
}
.recipe-modal-content .form-group {
  margin-bottom: 1rem;
}
.recipe-modal-content .form-group label {
  display: block;
  margin-bottom: 0.5rem;
}
.recipe-modal-content .form-group input,
.recipe-modal-content .form-group textarea {
  display: block;
  width: 100%;
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-bottom: 1rem;
}
.recipe-modal-content .form-group textarea {
  height: 100px;
  resize: none;
}
.recipe-modal-content button[type="submit"] {
  margin-right: 1rem;
}
</style>