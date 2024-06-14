<template>
  <div class="div-form">
    <h1>Ajoutez une tâche</h1>
    <form @submit.prevent="soumissionFormulaire">
      <input type="text" required v-model="modif.text" v-if="modif.text" />
      <input type="text" required v-model="inputValue" v-else />
      <br />
      <button v-if="modif.text">Modifier</button>
      <button v-else>Ajouter</button>
    </form>
  </div>
</template>


<script setup>

import { ref, watch } from "vue";

const emit = defineEmits([
    'add-tache',
    "send-tache-modif"
]);

const props = defineProps({
    todoAModifier: Object,
});

let id = ref(0);
let todoArray = ref([]);
let inputValue = ref("");
let modif = ref({ text: "" })

watch(() => props.todoAModifier, (newVal) => {
    if(newVal && newVal.text){
        modif.value = { ...newVal };
    } else {
        modif.value = {text: ""};
    }
})


//Incrémentation et réinitialisation de la l'entrée input
function changeValue() {
  id.value++;
  inputValue.value = "";
}

function modifList() {
    emit("send-tache-modif", modif.value);
    modif.value = {text: ""};
    console.log(modif.value);
}

function addList() {

    if(inputValue.value.trim()){
        todoArray.value.push({
        id: id.value,
        text: `${inputValue.value}`,
        etat: false,
        });
        emit('add-tache', todoArray);
        changeValue();
    } else {
        inputValue.value = "";
    }
}


function soumissionFormulaire()
{
    if(modif.value && modif.value.text)
    {
        modifList();
    } 
    else {
        addList();
    }
}


</script>

<style scoped>
h1 {
  color: blueviolet;
}
</style>