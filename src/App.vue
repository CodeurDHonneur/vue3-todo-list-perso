<template>
  <div class="contain">
    <h1>Bienvenu(e) dans l'univers des tâches</h1>

    <div class="form-component">
     <!-- Formulaire d'ajout et de modification  -->
      <FormulaireVue 
      @add-tache="addList"
      :todoAModifier="todoModif"
      @send-tache-modif="modificationTodoFaite"
      />


     <!-- Tableau d'affichage de nos listes -->
     <TableauVue 
     :todoArray="todoArray"
     @change-etat="modifEtat"
     @sup-todo="supTodo"
     @modif-todo="recupTodoAModif"
     />

    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import FormulaireVue from "./Components/FormulaireVue.vue";
import TableauVue from "./Components/TableauVue.vue";


let todoArray = ref([]);
let todoModif= ref({});


const addList = function (list) {
  todoArray.value = list.value;
};

const modifEtat = function(list) {
  todoArray.value.map(todo => {
   if(todo.id  == list.id)
   todo = list
  })
}

const supTodo = function(list){
  todoArray.value = todoArray.value.filter(todo => todo.id !== list.id)
}

/**
 * Modification d'une tâche
 * Récupération de la liste à modifier et transmission au formulaire recupTodoAModif
 */

const recupTodoAModif = function (list) {
  todoModif.value = list;
}

function modificationTodoFaite(list){


  const index = todoArray.value.findIndex(todo => todo.id === list.id);

  if(index !== -1){
    todoArray.value[index] = list;
  }

 

}

</script>

<style>
/*********** Contain ***********/
.contain {
  width: 800px;
  background-color: white;
  height: 800px;
  text-align: center;
  box-shadow: 5px 5px 2px blueviolet, 10px 10px 2px burlywood,
    -5px -5px 2px burlywood, -10px -10px 2px blueviolet;
}

h1 {
  font-family: Cambria, Cochin, Georgia, Times, "Times New Roman", serif;
  font-size: 2.5em;
  color: burlywood;
  text-decoration: underline;
  width: 90%;
  margin: 0 auto;
}

/*********************************/

div.form-component {
  display: flex;
  flex-direction: column;
  align-items: center;
}

div.form-component > div.contain-table {
  width: 80%;
}

div.form-component > .div-form {
  width: 50%;
}

div.form-component > div.div-form input {
  width: 80%;
  height: 30px;
  padding-left: 15px;
}

div.form-component > div.div-form button {
  width: 30%;
  height: 20px;
  color: brown;
  font-weight: bold;
  background-color: lavender;
  border: none;
  cursor: pointer;
}

div.form-component > div.div-form button:hover {
  transition: 0.6s;
  transform: scale(1.1);
  color: burlywood;
  background-color: white;
}




.form-component {
  display: flex;
  gap: 15px;
  justify-content: center;
  align-items: center;
}

</style>