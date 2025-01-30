<script setup>
import { ref, computed, onMounted, watch } from 'vue'
import Header from './components/Header.vue';
import Footer from './components/Footer.vue';
import Guitarra from './components/Guitarra.vue';
import { db } from './data/guitarras.js'

const guitars =ref(db)
const carrito = ref([])
// const total = computed(() => {
//   let total = 0
//   carrito.value.forEach(g => {
//     total += g.precio * g.cantidad
//   })
//   return total
// })
const total = computed( () => {
   return carrito.value.reduce((t, g) => t + (g.cantidad * g.precio), 0)
  })

function guardaStorage(){
  localStorage.setItem('carrito', JSON.stringify(carrito.value))
}

function recuperaStorage(){
  const datos = localStorage.getItem('carrito')
  return datos ? JSON.parse(datos) : []
}

function agregarCarrito(guitar){
  const guitarId= carrito
    .value.findIndex(g => g.id === guitar.id)
  if(guitarId === -1 )
    carrito.value.push({ ...guitar, cantidad: 1 })
  else 
    carrito.value[guitarId].cantidad++
}

function agregaUno(id){
  const idCarrito=carrito.value.findIndex(g => g.id === id)
  if(carrito.value[idCarrito].cantidad>1)
    carrito.value[idCarrito].cantidad++
}

function quitaUno(id){
  const idCarrito=carrito.value.findIndex(g => g.id === id)
  if(carrito.value[idCarrito].cantidad>=1)
    carrito.value[idCarrito].cantidad--
  // else
  //   carrito.value[guitarId].cantidad=1
}

function quitaGuitarra(id){
  const idCarrito=carrito.value.findIndex(g=> g.id === id)
  carrito.value.splice(idCarrito, 1)
}

function vaciarCarrito(){
  carrito.value = []
}

onMounted(() => {
  carrito.value = recuperaStorage()
})

watch(carrito, guardaStorage, { deep:true })

</script>

<template>
  <Header 
    :carrito="carrito"
    :total="total"
    :guitar="guitars[3]"
    @agregar-carrito="agregarCarrito"
    @quita-uno="quitaUno"
    @quita-guitarra="quitaGuitarra"
    @vaciar-carrito="vaciarCarrito"
    @agrega-uno="agregaUno"
  />
    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div class="row mt-5">
            <Guitarra v-for="guitar in guitars"
            :key="guitar.id"
            :guitar="guitar"
            @agregar-carrito="agregarCarrito"
            />
        </div>
    </main>
  <Footer />
</template>