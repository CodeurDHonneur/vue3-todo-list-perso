<template>
  <div class="contain-table">
    <div class="toutCocher">
        <input type="checkbox" id="checkedAll">
        <label for="checkedAll">Tout chocher</label>
    </div>
    
    <table>
      <thead>
        <tr>
        
        <th>N°</th>
          <th>Avis</th>
          <th>
            <div  class="divTache">
                Tâche
            </div> 
        </th>
          <th>Etat</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody v-if="todoArray.length == 0">
        <tr>
          <td colspan="5">En attente de tâches...</td>
        </tr>
      </tbody>
      <tbody v-else>
        <tr v-for="(todo, index) in todoArray" :key="todo.id">
            <td>{{ ++index }}</td>
          <td>
            <input type="checkbox" @click="toggle(todo)" :checked="todo.etat" />
          </td>
          
          <td>{{ todo.text }}</td>
          <td>{{ !todo.etat ? "non traitée" : "traitée" }}</td>
          <td >
            <div class="td-icones">

           
            <span class="icone-up" @click="modif(todo)" v-if="!todo.etat">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
                stroke-width="1.5"
                stroke="currentColor"
                class="size-6"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  d="m16.862 4.487 1.687-1.688a1.875 1.875 0 1 1 2.652 2.652L6.832 19.82a4.5 4.5 0 0 1-1.897 1.13l-2.685.8.8-2.685a4.5 4.5 0 0 1 1.13-1.897L16.863 4.487Zm0 0L19.5 7.125"
                />
              </svg>
            </span>

            <span class="icone-sup" @click="supp(todo)">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
                stroke-width="1.5"
                stroke="currentColor"
                class="size-6"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  d="m20.25 7.5-.625 10.632a2.25 2.25 0 0 1-2.247 2.118H6.622a2.25 2.25 0 0 1-2.247-2.118L3.75 7.5m6 4.125 2.25 2.25m0 0 2.25 2.25M12 13.875l2.25-2.25M12 13.875l-2.25 2.25M3.375 7.5h17.25c.621 0 1.125-.504 1.125-1.125v-1.5c0-.621-.504-1.125-1.125-1.125H3.375c-.621 0-1.125.504-1.125 1.125v1.5c0 .621.504 1.125 1.125 1.125Z"
                />
              </svg>
            </span>
        </div>
          </td>
        </tr>
      </tbody>
      <tfoot v-if="todoArray.length !== 0">
        <tr>
          <th colspan="2">
            Total : <span class="mettreEnGras">{{ todoArray.length }}</span>
          </th>

          <th colspan="3" >
            <div class="recapTraitement">
                <span>Traîtées :  <span class="mettreEnGras">{{  nbrTraitees }}</span></span>
            <span>Non traîtées :  <span class="mettreEnGras">{{  nbrNonTraitees }}</span></span>
            </div>
          </th>
        </tr>
        <tr>
          <th colspan="5">
            <div class="container-button-tableau">
                <div> <input type="radio" id="toutAfficher" name="filtrage"> <label for="toutAfficher" >Tout afficher</label> </div>
                <div> <input type="radio" id="traitees" name="filtrage"> <label for="traitees" >Traîtées</label></div>
                <div> <input type="radio" id="nonTraitees" name="filtrage"> <label for="nonTraitees" >Non traîtées</label> </div>
            </div>
          </th>
        </tr>
      </tfoot>
    </table>
  </div>
</template>

<script setup>
import { ref, watch } from "vue";

let nbrTraitees = ref(0);
let nbrNonTraitees = ref(0);



const emit = defineEmits([
    "change-etat",
    "sup-todo",
    "modif-todo"
]);


const props = defineProps({
    todoArray: Array
});


watch(() => props.todoArray, (array) => {
    console.log(props.todoArray, array);
    nbrTraitees = array.filter(todo => todo.etat == true).length;
    nbrNonTraitees = array.filter(todo => todo.etat == false).length;
}, {deep: true});



//Modification de l'état en fonction du checkbox
function toggle(todo){
  let  list = todo;
    if(list.etat){
    list.etat =  false;

    } else {
        list.etat =  true;
    }

    emit("change-etat", list);
}



//Modification d'un élément
function modif(todo) {
    console.log(todo);
    emit("modif-todo", todo);
}

//Suppression d'un élément
function supp(todo) {
    emit("sup-todo", todo);
}

</script>

<style scoped>

/*********** tableau  ***********/
table {
  border-collapse: collapse;
  border: solid 1px;
  width: 100%;
}

table th,
table td {
  padding-inline: 15px;
  padding-block: 5px;
  border: solid 1px;
}


.td-icones {
  display: flex;
  justify-content: space-around;
}

.td-icones span{
 cursor: pointer;
}

.icone-sup svg {
  color: red;
  width: 20px;
}

.icone-up svg{
  color: blue;
  width: 20px;
}

.recapTraitement {
    display: flex;
    justify-content: space-around;
}

.container-button-tableau {
    display: flex;
    justify-content: space-around;
    
}
.container-button-tableau button{
   border: none;
   background-color: white;
    font-size: 1em;
    font-weight: bold;
}

.divTache {
    width: 400px;
}

.mettreEnGras {
    font-weight: bold;
}
/*********************************/
</style>