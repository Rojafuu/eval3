<template>
  <div class="container">
    <div class="row">
      <!-- Productos disponibles -->
      <div class="col-md-6">
        <h3>Productos Disponibles</h3>
        <div
          class="producto"
          v-for="producto in productos"
          :key="producto.nombre"
        >
          <img :src="producto.imagen" alt="producto" />
          <div class="producto-info">
            <p>
              <strong>{{ producto.nombre }}</strong> - Precio $:
              {{ producto.precio.toLocaleString() }}
            </p>
            <button @click="agregarAlCarrito(producto)">
              🛒 Agregar al Carrito
            </button>
          </div>
        </div>
      </div>

      <!-- Carrito -->
      <div class="col-md-6">
        <h3>Productos en el Carrito</h3>
        <div
          class="item-carrito"
          v-for="item in carrito"
          :key="item.nombre"
        >
          <img :src="item.imagen" alt="carrito" />
          <div class="item-info">
            <p>
              <strong>{{ item.nombre }}</strong> - Cantidad: {{ item.cantidad }}
            </p>
            <button class="btn-danger" @click="removerDelCarrito(item)">
              🗑️ Remover del Carrito
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Total -->
    <h4 class="total">Total a Pagar: ${{ total.toLocaleString() }}</h4>

  <div v-if="mostrarModal" class="modal-overlay">
    <div class="modal-content">
      <p>{{ mensajeModal }}</p>
      <button @click="mostrarModal = false">Cerrar</button>
    </div>
  </div>
  </div>
</template>

<script>
import audifonoImg from './assets/img/audifonos.png'
import mouseImg from './assets/img/mouse.png'
import tecladoImg from './assets/img/teclado.png'
import gabineteImg from './assets/img/gabinete.png'
import pantallaImg from './assets/img/pantalla.png'
import sillaImg from './assets/img/silla.png'

export default {
  data() {
    return {
      productos: [
        { nombre: 'Audífonos', precio: 30000, stock: 3, imagen: audifonoImg },
        { nombre: 'Mouse', precio: 20000, stock: 5, imagen: mouseImg },
        { nombre: 'Teclado', precio: 15000, stock: 10, imagen: tecladoImg },
        { nombre: 'Gabinete', precio: 35000, stock: 4, imagen: gabineteImg },
        { nombre: 'Pantalla', precio: 175000, stock: 3, imagen: pantallaImg },
        { nombre: 'Silla', precio: 150000, stock: 2, imagen: sillaImg }
      ],
      carrito: [],
      mostrarModal: false,
      mensajeModal: '',
    }
  },
  computed: {
    total() {
      return this.carrito.reduce(
        (suma, item) => suma + item.precio * item.cantidad,
        0
      )
    }
  },
  methods: {
    agregarAlCarrito(producto) {
      const item = this.carrito.find(p => p.nombre === producto.nombre)
      const cantidadActual = item ? item.cantidad : 0

      if (cantidadActual < producto.stock) {
        if (item) {
          item.cantidad++
        } else {
          this.carrito.push({ ...producto, cantidad: 1 })
        }
      } else {
          this.mensajeModal = 'No hay más unidades disponibles en stock'
          this.mostrarModal = true
  }
    },
    removerDelCarrito(item) {
      this.carrito = this.carrito.filter(p => p.nombre !== item.nombre)
    }
  }
}
</script>


<style>
.container {
  max-width: 1000px;
  margin: 10px auto;
  font-family: Arial, sans-serif;
  color: #222;
  font-size: 14px;
}

.row {
  display: flex;
  gap: 10px;
}

.col-md-6 {
  flex: 1;
  padding: 0 10px;
}

h3 {
  text-align: center;
  font-weight: bold;
  font-size: 22px;
  margin-bottom: 15px;
}

.producto, .item-carrito {
  display: flex;
  gap: 10px;
  margin-bottom: 10px;
}

.producto-info, .item-info {
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 6px;
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 999;
}

.modal-content {
  background: white;
  padding: 20px 30px;
  border-radius: 8px;
  box-shadow: 0 5px 15px rgba(0,0,0,0.3);
  text-align: center;
  max-width: 300px;
}


.producto img, .item-carrito img {
  width: 50px;
  height: 50px;
  object-fit: contain;
  border-radius: 4px;
  border: 1px solid #ddd;
}

button {
  background-color: #6a5acd; 
  color: white;
  padding: 4px 12px;
  border: none;
  border-radius: 4px;
  font-size: 13px;
  cursor: pointer;
  align-self: flex-start; 
  transition: background-color 0.3s;
}

button:hover {
  background-color: #5a4abf;
}

button.btn-danger {
  background-color: #6a5acd;
}

button.btn-danger:hover {
  background-color: #5a4abf;
}

.total {
  text-align: left;
  font-weight: bold;
  margin-top: 10px;
  font-size: 25px;
}
</style>
