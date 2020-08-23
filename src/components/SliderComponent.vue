<template>
  <div class="carousel-view">
    <h2>Title</h2>
    <transition-group class="carousel" tag="div">
      <div
        v-for="product in products"
        class="slide"
        @click="openModal(product.id)"
        :key="product.id"
      >
        <img :src="product.img" :alt="product.title" :title="product.title" />
        <div class="desc">
          <h4>{{ product.title }}</h4>
          <span class="price" v-if="product.stock">
            {{ product.price | priceNumber }}
          </span>
          <span class="stock" v-if="!product.stock">Out of stock</span>
        </div>
      </div>
    </transition-group>
    <div class="carousel-controls">
      <a class="carousel-controls__button" @click="prev">&#9664;</a>
      <a class="carousel-controls__button" @click="next">&#9654;</a>
    </div>

    <div class="modal" v-if="product" @click="closeModal">
      <div class="modal-container">
        <div class="modal-image">
          <img :src="product.img" :alt="product.title" :title="product.title" />
        </div>

        <div class="modal-dados">
          <button @click="product = false" class="close">X</button>
          <span>{{ product.price | priceNumber }}</span>
          <h2>{{ product.title }}</h2>
          <p>{{ product.description }}</p>
          <span v-if="!product.stock" class="product-stock">Esgotado</span>

          <div class="contact-container">
            <a href="https://google.com" class="contact" target="_blank">
              <img src="@/assets/whatsapp.svg" alt="WhatsApp" />
              <span>WhatsApp</span>
            </a>
            <a href="https://google.com" class="contact" target="_blank">
              <img src="@/assets/mail.svg" alt="Email" />
              <span>E-mail</span>
            </a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { api } from "@/services.js";

export default {
  name: "SliderComponent",
  data() {
    return {
      products: null,
      product: false,
    };
  },

  methods: {
    next() {
      const first = this.products.shift();
      this.products = this.products.concat(first);
    },

    prev() {
      const last = this.products.pop();
      this.products = [last].concat(this.products);
    },

    getProducts() {
      api.get("/product").then((r) => {
        this.products = r.data;
      });
    },

    openModal(id) {
      this.products.forEach((value) => {
        let itemNow = value.id;
        if (itemNow == id) {
          this.product = value;
        }
        window.scrollTo({
          top: 0,
          behavior: "smooth",
        });
      });
    },

    closeModal({ target, currentTarget }) {
      if (target == currentTarget) {
        this.product = false;
      }
    },
  },

  filters: {
    priceNumber(value) {
      return value.toLocaleString("pt-BR", {
        style: "currency",
        currency: "BRL",
      });
    },
  },

  created() {
    this.getProducts();
  },
};
</script>

<style scoped>
.carousel-view {
  display: flex;
  flex-direction: column;
  /* align-items: center; */
  max-width: 1440px;
  margin-top: 40px;
  padding: 0 80px;
}
.carousel {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  overflow: hidden;
  width: 100%;
  margin-top: 40px;
  padding-bottom: 30px;
  border-bottom: 3px solid #000000;
}

h2 {
  border-bottom: 3px solid #000000;
  color: #000000;
  padding-bottom: 10px;
}
h4 {
  font-size: 1rem;
  margin-bottom: 20px;
}
h4:hover {
  text-decoration: underline;
}
.price {
  font-size: 2rem;
  color: #000000;
  margin-bottom: 30px;
}
img {
  margin-bottom: 20px;
}
.slide {
  flex: 0 0 22%;
  height: 22%;
  margin: 1em;
  display: flex;
  flex-direction: column;
  justify-content: center;
  transition: transform 0.3s ease-in-out;
  cursor: pointer;
}
.desc {
  display: flex;
  flex-direction: column;
  color: #000000;
}
.slide:first-of-type {
  opacity: 0;
}
.slide:last-of-type {
  opacity: 0;
}
.stock {
  font-size: 0.85rem;
  color: red;
}
.carousel-controls {
  text-align: center;
  margin: 15px 0 30px 0;
}

.carousel-controls__button {
  cursor: pointer;
  padding: 0 5px;
  color: #000000;
  margin: 10px;
  font-size: 30px;
}

/*Modal*/
.modal {
  display: flex;
  flex-direction: column;
  align-items: center;
  position: absolute;
  top: 0px;
  left: 0px;
  width: 100%;
  padding: 80px;
}

.modal-container {
  position: relative;
  background: linear-gradient(to right, transparent 200px, white 200px);
  z-index: 1;
  display: grid;
  align-items: flex-start;
  grid-gap: 50px;
  padding: 0 50px 50px 0;
  animation: fadeIn 0.3s forwards;
}
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translate3d(50px, 0, 0);
  }
  to {
    opacity: 1;
    transform: translate3d(0px, 0, 0);
  }
}

.modal-image {
  grid-column: 1;
  margin-top: 50px;
  box-shadow: 0px 3px 4px rgba(0, 0, 0, 0.1), 0px 4px 10px rgba(0, 0, 0, 0.2);
  border-radius: 15px;
}

.modal-image img {
  max-width: 300px;
  display: block;
  border-radius: 15px;
}
.modal-dados {
  grid-column: 2;
  max-width: 600px;
  padding-top: 40px;
}

.modal-dados h2 {
  font-size: 3rem;
  padding-bottom: 0;
  padding-top: 5px;
}

.modal-dados p {
  padding-top: 20px;
}

.close {
  border-radius: 50%;
  border: 2px solid #000;
  width: 40px;
  height: 40px;
  position: absolute;
  top: -10px;
  right: -10px;
  font-size: 1rem;
  cursor: pointer;
  box-shadow: 0px 3px 4px rgba(0, 0, 0, 0.1), 0px 4px 10px rgba(0, 0, 0, 0.2);
}

.product-stock {
  font-size: 0.75rem;
  color: red;
  display: block;
  margin-top: 20px;
}

/*Contact*/
.contact-container {
  display: flex;
  margin: 20px 0 10px 0;
}

.contact {
  display: flex;
  justify-content: center;
  align-items: center;
  color: #000000;
  border: 2px solid #000000;
  min-width: 100px;
  padding: 10px 20px;
  font-weight: bold;
  margin: 0 10px 0 0;
}
.contact img {
  width: 20px;
  height: 20px;
  margin: 0 5px 0 0;
}
</style>
