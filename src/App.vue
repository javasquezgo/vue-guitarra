<script setup>
import { ref, reactive, onMounted, watch } from "vue";
import { db } from "./data/guitarras";
import Header from "./components/Header.vue";
import Guitarra from "./components/Guitarra.vue";
import Footer from "./components/Footer.vue";

const guitarras = ref([]);
const guitarra = ref({});
const carrito = ref([]);

watch(
  carrito,
  () => {
    guardarLocalStorage;
  },
  {
    deep: true,
  }
);

onMounted(() => {
  guitarras.value = db;
  guitarra.value = db[3];

  const carritoStorage = localStorage.getItem("carrito");
  if (carritoStorage) {
    carrito.value = JSON.parse(carritoStorage);
  }
});

const guardarLocalStorage = () => {
  localStorage.setItem("carrito", JSON.stringify(carrito.value));
};

const agregarCarrito = (guitarra) => {
  if (carrito.value.includes(guitarra)) {
    guitarra.cantidad++;
  } else {
    guitarra.cantidad = 1;
    carrito.value.push(guitarra);
  }

  guardarLocalStorage();
};

const decrementarCantidad = (producto) => {
  console.log("Menos");
  //Esto es una forma de validad que no pase de cierta cantidad
  if (producto.cantidad <= 1) return;
  producto.cantidad--;
};

const aumentarCantidad = (producto) => {
  console.log("Más");
  //Esto es una forma de validad que no pase de cierta cantidad
  if (producto.cantidad >= 6) return;
  producto.cantidad++;
};

const eliminarProducto = (idGuitarra) => {
  const indexToDelete = carrito.value.findIndex(
    (guitarra) => guitarra.id === idGuitarra
  );
  carrito.value.splice(indexToDelete, 1);
};
</script>

<template>
  <Header
    :carrito="carrito"
    :guitarra="guitarra"
    @agregar-carrito="agregarCarrito"
    @decrementar-cantidad="decrementarCantidad"
    @aumentar-cantidad="aumentarCantidad"
    @eliminar-producto="eliminarProducto"
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
