<template>
  <main-header />
  <sub-header
    heading="#stayhome"
    subHeading="Save more with coupons up to 70% off"
  />
  <div class="container">
    <div class="product__container" v-if="loaded">
      <product-card
        v-for="(product, index) in products"
        :key="index"
        :productId="product._id"
        :productName="product.name"
        :brand="product.brand"
        :price="product.price"
        :currency="product.currency"
        :ratings="product.rating"
        :image_url="product.images[0]"
        :in_stock="product.in_stock"
      />
    </div>
    <product-preloader v-else> Loading Products... </product-preloader>
  </div>
  <page-index />
  <main-footer />
</template>


<script>
import SubHeader from "@/components/SubHeader.vue";
import ProductCard from "@/components/home_components/cards/ProductCard.vue";
import PageIndex from "@/components/PageIndex.vue";
import MainHeader from "@/components/MainHeader.vue";
import MainFooter from "@/components/MainFooter.vue";

import axios from "axios";
import ProductPreloader from "@/components/ProductPreloader.vue";

export default {
  name: "ShopView",
  components: {
    SubHeader,
    ProductCard,
    PageIndex,
    MainHeader,
    MainFooter,
    ProductPreloader,
  },
  data() {
    return {
      products: [],
      loaded: false,
    };
  },
  async mounted() {
    let res1 = await axios.get("https://thegorana.herokuapp.com/products");
    let res2 = await axios.get(
      "https://thegorana.herokuapp.com/products/?page=2"
    );
    this.featured = res1.data.results.slice(0, 8);
    this.newProducts = res2.data.results.slice(0, 8);
    this.products = this.featured.concat(this.newProducts);
    this.loaded = true;
  },
};
</script>

