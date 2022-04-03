
<template>
  <div class="card my-5 shadow p-3 mb-5 bg-white rounded col border-warning" style="height:41rem;">
    <div class="card-body">
      <img :src="product.media.source" :alt="product.name" class="card-img-top">
      <h4 class="title text-center card-title display-5">{{product.name}}</h4>
      <p class="description text-center card-text display-5" v-html="product.description"></p>
      <p
        class="text-center text-muted card-subtitle display-5 price"
      >{{product.price.formatted_with_symbol}}</p>
      <button class="btn py-2 px-4 btn-outline-warning rounded-pill"
              
              @click="$emit('add-to-cart', product)">Add to Cart</button>
    </div>
  </div>
</template>

<script>
export default {
  props: ["product"]
};
</script>

<style>
body {
  letter-spacing: 2px;
}
.card h4 {
  padding-top: 20px;
  font-size: 20px;
  font-weight: 700;
  font-family: futura;
  line-height: 32px;
}

.card p {
  padding-top: 5px;
}

.card button {
  background-color: #FF9999;
  color: white;
  cursor: pointer;
  border: 0px solid #FF9999;
  transition: all 0.3s ease-in-out;
  justify-content: flex-end;
  position: relative;
  display: flex;
  margin: 0 auto;
  line-height: 32px;
}

.card button:hover {
  background-color: #FF0066
;
  color: #fff;
  border: 1px solid #FF0066
;
}

@media only screen and (max-width: 400px) {
  .card {
    height: 48rem;
  }
}
</style>