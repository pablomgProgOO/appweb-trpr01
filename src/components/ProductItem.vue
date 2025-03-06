<script setup lang="ts">
import { defineProps, defineEmits } from "vue";
import type { Ball } from "../scripts/Product";

const props=defineProps<{ product: Ball }>();
const emit = defineEmits(["select", "edit", "duplicate", "delete"]);
const buy=()=>{
    props.product.Stock+=1
}
const sell=()=>{
    props.product.Stock-=1
}
</script>

<template>
  <tr class="card-product">
    <td>{{ product.Name }}</td>
    <td v-if="product.Stock>0">{{ product.Stock }}</td>
    <td v-else class="low-stock">Produit sans stock</td>
    <td>
      <button @click="$emit('select', product)">Voir</button>
      <button @click="$emit('edit', product)">Modifier</button>
      <button @click="$emit('duplicate', product)">Dupliquer</button>
      <button @click="$emit('delete', product)">Supprimer</button>
      <button @click="buy">Acheter</button>
      <button v-if="product.Stock>0" @click="sell">Vendre</button>
    </td>
  </tr>
</template>

<style scoped>
.low-stock {
  color: rgb(115, 17, 4);
  font-weight: bold;
}
button {
  margin: 2px;
  padding: 5px;
  cursor: pointer;
}


</style>
