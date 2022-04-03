<template>
  <div id="app">

<!-- -------header navbar----------- -->

    <b-navbar
      toggleable="lg"
      class="py-3 shadow p-3 mb-0"
      style="background-color: #FFFFFF"
    >
      <b-navbar-brand
        href="#"
        class="mx-5"
        style="font-family: arial black; color: #ffc233; font-size: 37px"
        ><b>BUZZ STOP</b><b><p style="font-family: arial black; color: #FF9999; font-size: 24px">
        CONCEPT STORE</p></b></b-navbar-brand
      >

      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

      <b-collapse id="nav-collapse" class="mx-5" is-nav>
        <b-navbar-nav
          class="ml-auto my-2"
          style="font-family: futura; color: #FF6666; text-align: center"
        >
          <b-nav-item href="#">About Us</b-nav-item>
          <hr />
          <b-nav-item href="#">Our Collections</b-nav-item>
          <hr />
          <b-nav-item href="#">Our Products </b-nav-item>
          <hr />
          <b-nav-item href="#">Contact Us</b-nav-item>
          <hr />

          <div style="justify-content: right">
            <b-button
              pill variant="outline-light"
              v-b-modal.modal-1
              style="background-color: #ffc233"
              class="mx-2 my-1 px-3"
            >
              Log-In</b-button
            >
             <b-modal id="modal-1" title="Log In">
                <label for="text-email">Email</label>
                 <b-form-input id="text-email" type="email" placeholder="Enter email" required
                 ></b-form-input>
                 <b-form @submit.stop.prevent>
                 <label for="text-password">Password</label>
                 <b-form-input type="password" placeholder="Enter password" id="text-password" aria-describedby="password-help-block">
                 </b-form-input>
                 </b-form>
             </b-modal>


            <b-button
              pill variant="outline-light"
              style="background-color: #FF9999"
              class="mx-2 my-1 px-3"
            >
              Sign Up</b-button
            >
          </div>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
    <b-img 
        src="https://static.dezeen.com/uploads/2018/02/likeshop-showroom-eduard-eremchuk-interior-retail-russia-_dezeen_2364_hero_a.jpg"
        fluid
        alt="Responsive image"
      ></b-img>

    <!-- -------View Cart----------- -->

    <div class="header">
      <b-button   pill variant="outline-light"
              class="mx-2 my-1 px-3" @click="toggleCart">View Cart</b-button>
      <p v-if="cartItems">{{ cart.total_items }}</p>
    </div>

    <!-- -------Product Card----------- -->

    <Cart
      :items="cartItems"
      v-if="displayCart"
      @remove-from-cart="removeFromCart"
      @close="emitClose"
    />
    <div class="container mx-auto px-4">
      <div class="flex mb-4">
        <div class="row">
          <div v-for="product in products" :key="product.id" class="col-sm-4">
            <Product :product="product" @add-to-cart="addToCart(product)" />
          </div>
        </div>
      </div>
    </div>
    <!-- -------footer navbar----------- -->

    <b-navbar
      toggleable="lg"
      class="py-3 shadow p-3 mb-0"
      style="
        background-color: #FF9999;
        justify-content: center;
        text-align: center;
      "
    >
      <b-navbar-brand
        href="#"
        class="mx-5 "
        style="font-family: futura; color: #ffffff; font-size: 17px"
        ><b>Buzz Stop Concept Store</b>
        <p>2022</p>
      </b-navbar-brand>
    </b-navbar>
  </div>

</template>

<script>
//Import Commerce dependency
import Commerce from "@chec/commerce.js";
import Product from "@/components/Product.vue";
import Cart from "@/components/Cart.vue";

// Initialize store with public key
const commerce = new Commerce(
  "pk_168757b95ff55c066b59b9e93c48a2b0e7bc1b97c798b",
  true
);

export default {
  name: "app",
  components: {
    Product,
    Cart,
  },

  data() {
    return {
      products: [],
      cart: [],
      displayCart: false,
    };
  },

  created() {
    commerce.products
      .list()
      .then((response) => {
        // Success
        this.products = response.data;
      })
      // Error
      .catch((error) => {
        console.log(error);
      });

    //Invoke commerce cart method to retrieve cart in session
    commerce.cart
      .retrieve()
      .then((response) => {
        //Successful response
        this.cart = response;
      })
      //Error
      .catch((error) => {
        // eslint-disable-next-line
        console.log(error);
      });
  },

  methods: {
    //Add products to cart
    addToCart(product) {
      commerce.cart
        .add({
          id: product.id,
          quantity: 1,
        })
        .then((response) => {
          this.cart = response.cart;
        });
    },

    toggleCart() {
      this.displayCart = !this.displayCart;
    },

    emitClose($event) {
      this.$emit("close", $event);
      this.displayCart = false;
    },

    removeFromCart(lineItemId) {
      commerce.cart
        .remove(lineItemId)
        .then((response) => {
          this.cart = response.cart;
        })
        .catch((error) => {
          //eslint-disable-next-line
          console.log(error);
        });
    },
  },

  computed: {
    cartItems() {
      return this.cart ? this.cart.line_items : [];
    },
  },
};
</script>

<style>
.header {
  display: flex;
  justify-content: flex-end;
  position: relative;
  padding-top: 30px;
  padding-right: 20px;
}

.header button {
  text-decoration: none;
  background-color: #FF9999;
  color: #fff;
  padding: 0 30px;
  cursor: pointer;

  transition: 0.3s all ease-in-out;
  width: 150px;
  height: 50px;
}

.header button:hover {
  background-color: #FF0066;
  color: white;
  line-height: 30px;
}

.header p {
  padding: 10px 10px;
}
</style>