<script setup lang="ts">
import { defineProps, defineEmits } from "vue";
import type { Ball } from "../scripts/Product";
import ProductItem from "./ProductItem.vue";

defineProps<{ products: Ball[] }>();
const emit = defineEmits(["select", "edit", "duplicate", "delete"]);
</script>

<template>
  <div v-if="products.length>0">
  <table>
    <thead>
      <tr>
        <th>Nom</th>
        <th>Stock</th>
        <th>Actions</th>
      </tr>
    </thead>
    <tbody>
      <ProductItem class="" v-for="product in products" :key="product.id" :product="product"
          @select="$emit('select', product)"
          @edit="$emit('edit', product)"
          @duplicate="$emit('duplicate', product)"
          @delete="$emit('delete', product)"
      />
    </tbody>
  </table>
  </div>
  <div v-else>
    <h2>
      Aucun produit
    </h2>
  </div>
</template>
<style>
body {
    background-color: #f5f5dc; /* Beige background */
    color: #006400; /* Dark green text */
}
</style>