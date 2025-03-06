<script setup lang="ts">
import {ref,defineProps, defineEmits } from "vue";
import { type Ball } from "../scripts/Product";
const emit = defineEmits(["clear"]);
const props= defineProps<{ product: Ball}>();
const localProduct = ref(props.product);
const clear=()=>
{
    emit("clear");
}
</script>

<template>
  <div class="card">
    <h2>Nom: {{ localProduct.Name }}</h2>
    <h5>Description:{{ localProduct.Description }}</h5>
    <h5>Stock : {{ localProduct.Stock }}</h5>
    <button @click="clear">Retour</button>
  </div>
</template>
