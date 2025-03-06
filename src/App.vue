<script setup lang="ts">
import { ref } from "vue";
import ListView from "./components/ListView.vue";
import Form from "./components/Form.vue";
import ProductView from "./components/ProductView.vue";
import { type Ball, NullBall } from "./scripts/Product";
import 'bootstrap';
const products = ref<Ball[]>([]);
const uses=["Ajouter","Modifier","Dupliquer","Supprimer"];
const selectedProduct = ref<Ball>(NullBall);
const selectedViewProduct = ref<Ball>(NullBall);
const selectedUse = ref<String>(uses[0]);
const isFormVisible = ref(false);
const isViewVisible= ref(false);
const selectProductDelete = (product: Ball) => {
  selectedProduct.value = product;
  selectedUse.value=uses[3];
  isFormVisible.value = true

};
const selectProductEdit = (product: Ball) => {
  selectedProduct.value = product;
  selectedUse.value=uses[1];
  isFormVisible.value = true
};
const selectProductDuplicate = (product: Ball) => {
  selectedProduct.value = product;
  selectedUse.value=uses[2];
  isFormVisible.value = true

};
const selectAdd = () => {
  selectedProduct.value = NullBall;
  selectedUse.value=uses[0];
  isFormVisible.value = true

};

const selectViewProduct = (product: Ball) => {
  selectedViewProduct.value = product;
  isViewVisible.value=true
};

function handleFormSubmit(product: Ball){
    products.value.push(product);
    isFormVisible.value = false;

};
//FindIndex
//https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex
const handleDelete = (product: Ball) => {
  products.value = products.value.filter((p) => p.id !== product.id);
  selectedProduct.value = NullBall;
  isFormVisible.value = false;
};
const handleEdit = (product: Ball) => {
    const index = products.value.findIndex((p) => p.id === product.id);
    if (index !== -1) products.value[index] = product;
    isFormVisible.value = false;
};
const clear=()=>{
  isViewVisible.value = false;
}
const exportCSV = () => {
  if (products.value.length === 0) {
    alert("Aucun produit à exporter !");
    return;
  }
//logique format csv
//https://stackoverflow.com/questions/14964035/how-to-export-javascript-array-info-to-csv-on-client-side
  let csvProducts = "ID,Nom,Description,Stock\n";

  products.value.forEach((product) => {
    const row = `${product.id},"${product.Name}","${product.Description}",${product.Stock}`;
    csvProducts += row + "\n";
  });
  //logique du download
  //https://stackoverflow.com/questions/74423711/save-csv-from-an-array-of-objects-in-vuejs-3
  const blob = new Blob([csvProducts], { type: "text/csv" });
  const url = URL.createObjectURL(blob);

  const a = document.createElement("a");
  a.href = url;
  a.download = "produits.csv";
  document.body.appendChild(a);
  a.click();
  //liberer la memoire
  //https://developer.mozilla.org/en-US/docs/Web/API/URL/revokeObjectURL_static
  document.body.removeChild(a);
  URL.revokeObjectURL(url);
};
</script>

<template>
  <header class="d-flex gap-2">
    <img src="../src/assets/Images/ballLogo.png" alt="Logo">
    <h1>Gestionnaire de Balles</h1>
  </header>
  <div>
    <button class="card" @click="selectAdd">Ajouter un produit</button>
    <button class="card" @click="exportCSV">Exporter en CSV</button>
    <ListView :products="products" @selectProduct="selectViewProduct" @delete-product="selectProductDelete" @duplicate-product="selectProductDuplicate" @edit-product="selectProductEdit" />
    <Form v-if="isFormVisible" :products="products" :selectedProduct="selectedProduct" :selected-usage="selectedUse" @submit="handleFormSubmit" @delete="handleDelete" @edit="handleEdit" @duplicate="" @cancel="isFormVisible = false" />
    <ProductView v-if="isViewVisible" :product="selectedViewProduct" @clear="clear"/>
  </div>
</template>
<style>
body {
    background-color: #f5f5dc; /* Beige background */
    color: #006400; /* Dark green text */
}
img{
  width: 100px;
  height: 100px;
}
</style>