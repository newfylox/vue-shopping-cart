<template>
  <div class="checkout-box">
    <div v-if="hasProduct && !defaultProfileId">
      <div>
        <p>
          <router-link :to="backTo">{{
            $t("shop.cart_checkout.back_to_products")
          }}</router-link>
        </p>
      </div>
      <div id="cart-checkout-title">
        <h4>{{ $t("shop.cart_checkout.title") }}</h4>
      </div>
      <ul class="checkout-list">
        <li
          v-for="(product, index) in getCartProducts"
          :key="`${index}-${product.sku}`"
          class="checkout-product"
        >
          <img
            :src="product.image_url"
            alt=""
            class="product-image img-fluid"
          />
          <h3 class="product-name">{{ product.name }}</h3>
          <span class="product-price">${{ product.price }}</span>
          <b-button variant="danger" @click="remove(index)"
            ><b-icon icon="trash" aria-label="Help"></b-icon
          ></b-button>
        </li>
      </ul>
      <h3 class="total">
        {{ $t("shop.cart_checkout.subtotal") }} ${{ subTotal }}
      </h3>
    </div>
    <div v-if="!hasProduct" class="checkout-message">
      <h3>{{ $t("shop.cart_checkout.no_products") }}</h3>
      <router-link :to="backTo">{{
        $t("shop.cart_checkout.back_to_products")
      }}</router-link>
    </div>
  </div>
</template>

<script>
import { mapGetters, mapActions } from "vuex";
import dropin from "braintree-web-drop-in";

export default {
  props: ["backTo"],
  created() {
    this.loadCartProducts();
  },
  computed: {
    ...mapGetters({
      getCartProducts: "shop/getCartProducts",
      hasProduct: "shop/hasProduct",
      defaultProfileId: "profile/defaultProfileId",
      subTotal: "shop/subTotal",
    }),
  },
  methods: {
    ...mapActions({
      loadCartProducts: "shop/loadCartProducts",
      removeProduct: "shop/removeProduct",
    }),
    remove(index) {
      this.removeProduct(index);
    },
  },
};
</script>

<style scoped>
.checkout-box {
  width: 100%;
  max-width: 900px;
  display: flex;
  flex-direction: column;
  margin: 50px auto;
  box-sizing: border-box;
  padding: 1em;
}

.checkout-list {
  padding: 0;
}

.checkout-product {
  display: grid;
  grid-template-columns: 1fr 3fr 2fr 0.5fr;
  background-color: #fff;
  box-shadow: 0px 0px 10px rgba(73, 74, 78, 0.1);
  border-radius: 5px;
  list-style: none;
  box-sizing: border-box;
  padding: 0.8em;
  margin: 1em 0;
}

.checkout-product * {
  place-self: center;
}
.product-image {
  grid-column: 1/2;
}

.product-name {
  box-sizing: border-box;
}

.product-price {
  font-size: 1.2em;
  font-weight: bold;
}

.product-remove {
  width: 25px;
  height: 25px;
  text-align: left;
  border-radius: 50%;
  border: 0;
  color: #fff;
  cursor: pointer;
}

.total {
  font-size: 2em;
  font-weight: bold;
  align-self: flex-end;
}

.checkout-message {
  font-size: 1.5em;
}
</style>
