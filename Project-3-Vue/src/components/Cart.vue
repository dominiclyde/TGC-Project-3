<template>
  <div class="modal">
    <div class="cart">
      <header class="cart-header">
        <slot class="header">
          <h3>Shopping Cart</h3>
          <button type="button" class="btn-x" @click="emitClose">x</button>
        </slot>
      </header>

      <div class="cart-body" v-for="item in items" :key="item.id">
        <slot name="body">
          <div class="cart-contents">
            <div class="item-name">{{ item.name }}</div>
            <span class="text-center">{{
              item.price.formatted_with_symbol
            }}</span>
            <b-button
              class="rounded-pill"
              name="btn-remove"
              @click="$emit('remove-from-cart', item.id)"
            >
              Remove
            </b-button>
          </div>
        </slot>
      </div>
      <div class="cart-footer">
        <slot name="footer">
          Total: ${{ cartTotal.toFixed(2) }}
          <b-button  type="button" class="btn-close rounded-pill" @click="emitClose">
            Proceed to Pay
          </b-button>
          <b-button type="button" class="btn-close rounded-pill" @click="emitClose">
            Close
          </b-button>
        </slot>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "cart-modal",

  props: ["items", "cart"],

  methods: {
    emitClose($event) {
      this.$emit("close", $event);
    },
  },

  computed: {
    cartTotal() {
      return this.items.reduce(
        (acc, item) => acc + Number(item.price.formatted),
        0
      );
    },
  },
};
</script>

<style>
.modal {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.3);
  display: flex;
  justify-content: center;
  align-items: center;
}

.cart {
  padding: 15px 20px;
  border-radius: 11px;
  background: #ffffff;
  box-shadow: 2px 2px 2px 1px rgba(73, 73, 73, 0.493);
  overflow-x: auto;
  display: flex;
  flex-direction: column;
  width: 500px;
  font-family: futura;
}

.cart-header,
.cart-footer {
  padding: 15px;
  display: flex;
}

.cart-header {
  border-bottom: 1px solid #eeeeee;
  color: black;
  justify-content: space-between;
  padding-top: 20px;
}

.cart-footer {
  border-top: 1px solid #eeeeee;
  justify-content: space-between;
}

.cart-body {
  position: relative;
  padding: 20px 10px;
}

.cart-contents {
  display: flex;
  justify-content: space-between;
}

.cart-contents button {
  height: 40px;
  font-size: 14px;
}

.cart-contents .item-name {
  width: 60%;
  font-weight: semi-bold;
}

.btn-x {
  border: none;
  font-size: 20px;
  padding: 20px;
  cursor: pointer;
  font-weight: bold;
  color: black;
  background: transparent;
  padding: 0;
  transition: all 0.2s ease-in-out;
}

.btn-x:hover {
  transform: scale(1.1);
  background: transparent;
  color: black;
  border: none;
}

.btn-close {
  padding: 10px;
  color: white;
  background: #FF9999;
  border: 1px solid #FF9999;
  border-radius: 10px;
  cursor: pointer;
}
</style>