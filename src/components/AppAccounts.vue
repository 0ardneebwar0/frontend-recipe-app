<style>
/* Add a border and some padding to the button */
.btn {
  border: 1px solid rgb(0, 0, 0);
  padding: 10px;
  background-color: rgb(34, 222, 222);
  margin: 0 10px;
  
}
/* Style the table with a striped pattern and hover effect */
.table {
  border-collapse: collapse;
  width: 100%;
}
.table-striped tbody tr:nth-of-type(odd) {
  background-color: #7c3c7fb6;
}
.table-hover tbody tr:hover {
  background-color: #c3373777;
}
.jumbot {
  background-image: url(recipe_01.jpg);
  background-size: cover;
  background-position: center;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 15vh;
}
.edit-btn {
  background-color: grey;
}
.delete-btn {
  background-color: darkred;
}
h1 {
text-align: center;
color: rgb(255, 255, 255);
}
</style>

<template>
  <div class="jumbotron vertical-center">
      <div class = "container">
          <div class = "row">
              <div class = "col-sm-12">
                <div class="jumbot">
                <h1 style="font-size: 70px;">R E C I P E S</h1>
                </div>

                  <hr />
                  <!-- Allert Message -->
        <!--<b-alert v-if="showMessage" variant="success" show>{{
          message
        }}</b-alert>-->
        <!-- b-alert v-if="error" variant="danger" show>{{ error }}</b-alert-->
   
                  <table class="table table-striped table-hover" style="background-color:beige;">
                    <colgroup>
                      <col style="width: 10%;" />
                      <col style="width: 30%;" />
                      <col style="width: 40%;" />
                      <col style="width: 10%;" />
                      <col style="width: 10%;" />
                    </colgroup>
                      <thead>
                          <tr>
                              <th scope = "col"> Food </th>
                              <th scope = "col"> Ingredients </th>
                              <th scope = "col"> Instructions </th>
                              <th scope = "col"> Favorite </th>
                              <th scope = "col"> Rating </th>
                          </tr>
                      </thead>
                      <tbody>
                          <tr v-for= "recipe in recipes" :key = "recipe.id">
                              <td> {{ recipe.name }} </td>
                              <td> {{ recipe.ingredients }} </td>
                              <td> {{ recipe.instructions }} </td>
                              <td style="text-align: center;">

                                <template>
                                  <p>{{ recipe.favorite ? '&#x2764' : '&#x2661' }}</p>
                                </template>
                                </td>
                                <td>
                                    <template v-if="recipe.ratings === 5">
                                        &#9733&#9733&#9733&#9733&#9733
                                    </template>
                                    <template v-if="recipe.ratings === 4">
                                        &#9733&#9733&#9733&#9733&#9734
                                    </template>
                                    <template v-if="recipe.ratings === 3">
                                        &#9733&#9733&#9733&#9734&#9734
                                    </template>
                                    <template v-if="recipe.ratings === 2">
                                        &#9733&#9733&#9734&#9734&#9734
                                    </template>
                                    <template v-if="recipe.ratings === 1">
                                        &#9733&#9734&#9734&#9734&#9734
                                    </template>
                            
                                 </td>
                              
                               <td> 
                                  <div class="btn-group btn-group-sm" role="group">
                                    <button
                                        type="button"
                                        style = "width: 60px;"
                                        class="btn btn-warning btn-sm edit-btn"
                                        v-b-modal.edit-recipe-modal
                                        @click="editRecipe(recipe)"
                                        >
                                        Edit
                                        </button>

                                        <button
                                            type="button"
                                            style = "width: 60px;"
                                            class="btn btn-danger btn-sm delete-btn"
                                            @click="deleteRecipe(recipe)"
                                            >
                                            Delete
                                            </button>
                                  </div>
                              </td>
                          </tr>
                      </tbody>
                  </table>
                  <button
              type="button"
              class="btn btn-success btn-sm"
              style="background-color: #50CAFE; float: right;"
              v-b-modal.recipe-modal
                    >
                    Add Recipe
                </button>

                  <footer class = "text-center">
                  </footer>
              
              </div>
          </div>
          <!-- Beginning of Modal for Create -->
              <div>   <b-modal 
              ref = "addRecipeModal"
              id="recipe-modal"
              title="ADD NEW RECIPE"
              hide-backdrop
              hide-footer
              >
              <b-form @submit="onSubmit" class="w-100">
              <b-form-group style="font-weight:bold; color:royalblue;"
                  id="name"
              label="Food Name:"
              label-for="name"
              >
              <b-form-input style="background-color: lightgoldenrodyellow"
              id="name"
              type="text"
              v-model="createRecipeForm.name"
              placeholder="Food Name"
              required
              >
              </b-form-input>
              </b-form-group>
              <b-form-group style="font-weight:bold; color:royalblue;"
              id="ingredients"
              label="Ingredients:"
              label-for="ingredients"
              >
              <b-form-input style="background-color: lightgoldenrodyellow"
              id="ingredients"
              type="text"
              v-model="createRecipeForm.ingredients"
              placeholder="Ingredients"
              required
              ></b-form-input>
              </b-form-group>
              <b-form-group style="font-weight:bold; color:royalblue;"
              id="instructions"
              label="Instructions:"
              label-for="instructions"
              >
              <b-form-input style="background-color: lightgoldenrodyellow"
              id="instructions"
              type="text"
              v-model="createRecipeForm.instructions"
              placeholder="Instructions"
              required
              ></b-form-input>
              </b-form-group>
              <b-form-group style="font-weight:bold; color:royalblue;"
              id="favorite"    
              label="Favorite:"
              label-for="form-edit-favorite-input"
              >
              <b-form-checkbox style="background-color: lightgoldenrodyellow"
                id="form-favorite-input"
                
                v-model="createRecipeForm.favorite"
                switch

              ></b-form-checkbox>
              
              </b-form-group>
              <b-form-group group style="font-weight:bold; color:royalblue;"
              id="ratings"
              label="Rating:"
              label-for="form-ratings-input"
              >
              <b-form-select style="background-color: lightgoldenrodyellow"
              id="form-ratings-input"
              type="integer"
              v-model="createRecipeForm.ratings"              
              required
            >
              <option value=1>
                &#9733;&#9734;&#9734;&#9734;&#9734;
              </option>
              <option value=2>
                &#9733;&#9733;&#9734;&#9734;&#9734;
              </option>
              <option value=3>
                &#9733;&#9733;&#9733;&#9734;&#9734;
              </option>
              <option value=4>
                &#9733;&#9733;&#9733;&#9733;&#9734;
              </option>
              <option value=5>
                &#9733;&#9733;&#9733;&#9733;&#9733;
              </option>
            </b-form-select>
              </b-form-group>
              <b-button type="submit" variant="primary">Submit</b-button>
              </b-form>
          </b-modal>
          <!-- End of Modal for Create Recipe-->

          <!-- Beginning of Modal for Edit Recipe-->
          <b-modal
          ref = "editRecipeModal"
          id="edit-recipe-modal"
          title="UPDATE THE RECIPE"
          hide-backdrop
          hide-footer
          >
          <b-form @submit="onSubmitUpdate" class="w-100">
          <b-form-group
          id="form-name-group"
          label="Recipe Name:"
          label-for="form-name-input"
          >
          <b-form-input style="background-color: lightgoldenrodyellow"
          id="form-name-input"
          type="text"
          v-model="updateRecipeForm.name"
          placeholder="Recipe Name"
          required
          >
          </b-form-input>
          </b-form-group>
          <b-form-group
          id="form-ingredients-group"
          label="Ingredients:"
          label-for="form-ingredients-input"
          >
          <b-form-input style="background-color: lightgoldenrodyellow"
          id="form-ingredients-input"
          type="text"
          v-model="updateRecipeForm.ingredients"
          placeholder="Ingredients"
          >
          </b-form-input>
          </b-form-group>
          <b-form-group 
          id="form-instructions-group"
          label="Instructions:"
          label-for="form-instructions-input"
          >
          <b-form-input style="background-color: lightgoldenrodyellow"
          id="form-instructions-input"
          type="text"
          v-model="updateRecipeForm.instructions"
          placeholder="Instructions"
          >
          </b-form-input>
          </b-form-group>
          <b-form-group
          id="form-favorite-group"
          label="Favorite:"
          label-for="form-favorite-input"
          >
          <b-form-checkbox style="background-color: lightgoldenrodyellow"
              id="form-favorite-input"
             
              v-model="updateRecipeForm.favorite"
              switch
            ></b-form-checkbox>
            
          </b-form-group>
          <b-form-group
          id="form-ratings-group"
          label="Rating:"
          label-for="form-ratings-input"
          >
          <b-form-select style="background-color: lightgoldenrodyellow"
              id="form-ratings-input"
              type="integer"
              v-model="updateRecipeForm.ratings"             
              required
            >
              <option value=1>
                &#9733;&#9734;&#9734;&#9734;&#9734;
              </option>
              <option value=2>
                &#9733;&#9733;&#9734;&#9734;&#9734;
              </option>
              <option value=3>
                &#9733;&#9733;&#9733;&#9734;&#9734;
              </option>
              <option value=4>
                &#9733;&#9733;&#9733;&#9733;&#9734;
              </option>
              <option value=5>
                &#9733;&#9733;&#9733;&#9733;&#9733;
              </option>
            </b-form-select>
          </b-form-group> 
          <b-button type="submit" variant="primary">Update</b-button>
          </b-form>
          </b-modal>
          <!-- End of Modal for Edit Recipe-->
      </div>
  </div>
</div>
</template>

<script>
  
  import axios from 'axios';
  export default
  {
    name: 'AppRecipes',
    data()
    {
        return {
            recipes: [],
            createRecipeForm: {
                name: '',
                ingredients: '',
                instructions: '',
                favorite: false,
                ratings: 1
            },

            updateRecipeForm: {
                name: '',
                ingredients: '',
                instructions: '',
                favorite: false,
                ratings: 1
            },
  
            showMessage: false,
            message: '',
  
        };
    },
    methods: {

        //GET Recipes
        RESTgetRecipes() {
        const path = `${process.env.VUE_APP_ROOT_URL}/recipes`;
        axios
            .get(path)
            .then((response) => {
                this.recipes = response.data.recipe;
                this.message = response.data.recipe;
                    // To actually show the message
                    this.showMessage = true;
                    // To hide the message after 5 seconds
                    setTimeout(() => {
                      this.showMessage = false;
                    }, 100000);
            }
            )
            .catch((error) => {
                console.log(error);
            });
        },

        //POST Recipes
        RESTcreateRecipe(payload){
            const path = `${process.env.VUE_APP_ROOT_URL}/recipe`;
            axios
                .post(path,payload)
                .then((response) => {
                    this.RESTgetRecipes();
                    this.message = "Recipe Created succesfully!";
                    // To actually show the message
                    this.showMessage = true;
                    // To hide the message after 5 seconds
                    setTimeout(() => {
                      this.showMessage = false;
                    }, 50000);
                })
                .catch((error) => {
                    console.log(error);
                    this.RESTgetRecipes()
                });
        },
  
        RESTupdateRecipe(payload, id){
            const path = `${process.env.VUE_APP_ROOT_URL}/recipe/${id}`;
            axios
                .put(path,payload)
                .then((response) => {
                    this.RESTgetRecipes();
  
                      this.message = "Recipe Updated Successfully"
                      this.showMessage = true;
                      setTimeout(() => {
                          this.showMessage = false;
                      }, 3000);
                      
                })
                .catch((error) => {
                    console.log(error);
                    this.RESTgetRecipes()
                });
        },
  
        RESTdeleteRecipe(id){
            const path = `${process.env.VUE_APP_ROOT_URL}/recipe/${id}`;
            axios
                .delete(path)
                .then((response) => {
                    this.RESTgetRecipes();
                    this.message = "Recipe Deleted Successfully"
                    this.showMessage = true;
                    set.TimeOut(() => {
                        this.showMessage = false;
                    }, 3000);
  
                })
                .catch((error) => {
                    console.log(error);
                    this.RESTgetRecipes()
                });
        },
  
        initForm(){
            this.createRecipeForm = {
                name: '',
                ingredients: '',
                instructions: '',
                favorite: false,
                ratings: 1
            };
            this.updateRecipeForm = {
                name: '',
                ingredients: '',
                instructions: '',
                favorite: false,
                ratings: 1
            };
  
        },
        onSubmit(e) {
            e.preventDefault(); 
            this.$refs.addRecipeModal.hide(); 
            const payload = {
                name: this.createRecipeForm.name,
                ingredients: this.createRecipeForm.ingredients,
                instructions: this.createRecipeForm.instructions,
                favorite: this.createRecipeForm.favorite,
                ratings: this.createRecipeForm.ratings
            };
            this.RESTcreateRecipe(payload);
            this.initForm();
        },
        onSubmitUpdate(e) {
          e.preventDefault(); 
          this.$refs.editRecipeModal.hide(); 
          const payload = {
              name: this.updateRecipeForm.name,
              ingredients: this.updateRecipeForm.ingredients,
              instructions: this.updateRecipeForm.instructions,
              favorite: this.updateRecipeForm.favorite,
              ratings: this.updateRecipeForm.ratings
          };
          this.RESTupdateRecipe(payload, this.updateRecipeForm.id);
          this.initForm();
      },
  
        deleteRecipe(recipe) {
            this.RESTdeleteRecipe(recipe.id);
        },
  
        editRecipe(recipe) {
            this.updateRecipeForm = recipe;
        },
    },
   
    created() {
        this.RESTgetRecipes();
    },
  };
  
  </script>
<style>
.vertical-center {
  min-height: 100%;  /* Fallback for browsers do NOT support vh unit */
  min-height: 100vh; /* These two lines are counted as one :-)       */
  display: flex;
  align-items: center;
}
</style>
