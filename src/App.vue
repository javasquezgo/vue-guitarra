<script setup>
import { ref, reactive, onMounted } from "vue";
import { db } from "./data/guitarras";
import Header from "./components/Header.vue";
import Guitarra from "./components/Guitarra.vue";
import Footer from "./components/Footer.vue";

const guitarras = ref([]);
const carrito = ref([]);

onMounted(() => {
  guitarras.value = db;
});

const agregarCarrito = (guitarra) => {
  if (carrito.value.includes(guitarra)) {
    guitarra.cantidad++;
  } else {
    guitarra.cantidad = 1;
    carrito.value.push(guitarra);
  }
};

const decrementarCantidad = (producto) => {
  console.log("Menos");
  if (producto.cantidad <= 1) return;
  producto.cantidad--;
};

const aumentarCantidad = (producto) => {
  console.log("Más");
  producto.cantidad++;
};
</script>

<template>
  <Header
    :carrito="carrito"
    @decrementar-cantidad="decrementarCantidad"
    @aumentar-cantidad="aumentarCantidad"
  />

  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>

    <div class="row mt-5">
      <Guitarra
        v-for="(guitarra, index) in guitarras"
        :key="index"
        :guitarra="guitarra"
        @agregar-carrito="agregarCarrito"
        @decrementar-cantidad="decrementarCantidad"
      />
    </div>
  </main>

  <Footer />
</template>

<style scoped>
</style>
