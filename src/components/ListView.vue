<script setup lang="ts">
import { ref, computed, defineProps, defineEmits } from "vue";
import List from "./List.vue";
import type { Ball } from "../scripts/Product";

const props = defineProps<{ products: Ball[] }>();
const emit = defineEmits(["selectProduct","deleteProduct","duplicateProduct","editProduct"]);

const searchQuery = ref("");

const filteredProducts = computed(() =>
  props.products.filter((product) =>
    product.Name.toLowerCase().includes(searchQuery.value.toLowerCase())
  )
);

function handleSelect(product: Ball) {
  emit("selectProduct", product);
};
function handleEdit(product: Ball) {
  emit("editProduct", product);
};
function handleDuplicate(product: Ball) {
  emit("duplicateProduct", product);
};
function handleDelete(product: Ball) {
  emit("deleteProduct", product);
};

</script>

<template>
  <div>
    <input class="form-control" v-model="searchQuery" placeholder="Rechercher un produit..." />
    <List :products="filteredProducts" @select="handleSelect" @edit="handleEdit" @delete="handleDelete" @duplicate="handleDuplicate" />
  </div>
</template>
<style>
body {
    background-color: #f5f5dc; /* Beige background */
    color: #006400; /* Dark green text */
}
</style>