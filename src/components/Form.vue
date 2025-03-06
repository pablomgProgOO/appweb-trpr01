<script setup lang="ts">
import { ref, defineProps, defineEmits } from "vue";
import type { Ball } from "../scripts/Product";
import 'bootstrap';
const props = defineProps<{ selectedProduct: Ball,selectedUsage: String,products: Ball[] }>();
const emit = defineEmits(["submit", "cancel", "delete","edit"]);
const localProduct = ref(props.selectedProduct);
const usage=ref(props.selectedUsage);
const isEditing=ref(false);
const isDeleting=ref(false);
const isDuplicating=ref(false);
const isAdding=ref(true);
const id=Date.now();
if(usage.value=="Modifier"){
    isEditing.value=true;
    isAdding.value=false;
}
if(usage.value=="Supprimer"){
    isDeleting.value=true;
    isAdding.value=false;
}
if(usage.value=="Dupliquer"){
    let newBall: Ball={
        id:id,
        Name:localProduct.value.Name,
        Description:localProduct.value.Description,
        Stock:localProduct.value.Stock
    }
    localProduct.value=newBall
    isDuplicating.value=true;
    isAdding.value=false;
}
if(usage.value=="Ajouter"){
    let newBall: Ball={
        id:id,
        Name:"",
        Description:"",
        Stock:0
    }
    localProduct.value=newBall;
}


function validateAdd(){
  if (!localProduct.value.Name.trim()) return "Le nom est requis.";
  if (localProduct.value.Stock < 0) return "Le stock ne peut pas être négatif.";
  return null;
};
//confirm box
//https://stackoverflow.com/questions/54156534/how-to-create-alert-confirm-box-in-vue
function handleDelete(){
    if (confirm(`Voulez-vous vraiment supprimer ${localProduct.value.Name} ?`)) {
    isDeleting.value=false;
    isAdding.value=true;
    emit("delete", localProduct.value);
  }
};


function handleEdit(){
  if (confirm(`Voulez-vous vraiment Modifier ${localProduct.value.Name} ?`)) {
    isEditing.value=false;
    isAdding.value=true;
    emit("edit", localProduct.value);
  }
};
function handleAdd(){
    isAdding.value=true;
    const error = validateAdd();
  if (error) {
    alert(error);
    return;
  }
  else {
    if(confirm(`Voulez-vous vraiment Ajouter ${localProduct.value.Name} ?`)){
    emit("submit", localProduct.value);
    }
  }
};

</script>

<template>
  <div class="container mt-4">
    <div class="card shadow-sm">
      <div class="card-header bg-success">
        <h2 class="mb-0 ">{{ usage }}</h2>
      </div>
      <div>
        <form>
          <div class="row mb-3 align-items-center">
            <label class="col-md-3 text-end fw-bold">Nom :</label>
            <div class="col-md-9">
              <input v-model="localProduct.Name" class="form-control" placeholder="Entrez le nom" />
            </div>
          </div>

          <div class="row mb-3 align-items-center">
            <label class="col-md-3 text-end fw-bold">Description :</label>
            <div class="col-md-9">
              <textarea v-model="localProduct.Description" class="form-control" rows="3" placeholder="Ajoutez une description"></textarea>
            </div>
          </div>

          <div class="row mb-3 align-items-center">
            <label class="col-md-3 text-end fw-bold">Stock :</label>
            <div class="col-md-9">
              <input type="number" v-model="localProduct.Stock" min="0" class="form-control" placeholder="Stock disponible" />
            </div>
          </div>

          <div class="d-flex gap-2">
            <button type="button" @click="$emit('cancel')" class="btn btn-secondary">Annuler</button>
            <button v-if="isAdding" @click="handleAdd" type="button" class="btn btn-primary">{{ usage }}</button>
            <button v-else-if="isDeleting" @click="handleDelete" type="button" class="btn btn-danger">{{ usage }}</button>
            <button v-else-if="isEditing" @click="handleEdit" type="button" class="btn btn-warning text-white">{{ usage }}</button>
            <button v-else-if="isDuplicating" @click="handleAdd" type="button" class="btn btn-info text-white">{{ usage }}</button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>


