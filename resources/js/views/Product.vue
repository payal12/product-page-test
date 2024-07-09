<template>
  <div class="product-page">
    <div v-if="isLoading"><Loader /></div>
    <div v-else class="product-section">
      <div class="image-gallery">
        <img :src="mainImage" alt="Main Product Image" class="main-image" />
        <div class="sub-images">
          <img
            v-for="(image, index) in productData.images"
            :key="index"
            :src="image"
            alt="Sub Image"
            :class="{
              'sub-image': true,
              selected: image === mainImage,
            }"
            @click="updateImageLink(index)"
          />
        </div>
      </div>
      <div class="product-details">
        <h1>{{ productData.name }}</h1>
        <p>{{ productData.description }}</p>
        <div>
          <span class="product-discounted"
            >$ {{ productData.price?.discounted }}.00</span
          ><span class="discount-badge"
            >{{ productData.discount?.amount }}%</span
          >
        </div>
        <div class="full-price">${{ productData.price?.full }}.00</div>
        <div class="d-flex">
          <div class="quantity-selector">
            <Button @click="decrementQuantity">-</Button>
            <div>{{ quantity }}</div>
            <Button @click="incrementQuantity">+</Button>
          </div>
          <Button buttonClass="add-to-cart d-flex"><span class="cart"></span><span>Add to cart</span></Button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
// Imports
import { ref, onMounted } from "vue";
import { useRoute, useRouter } from "vue-router";
import axios from "axios";
import Loader from "../components/Loader.vue";
import Button from "../components/Button.vue";

// Variable Declaration
const route = useRoute();
const router = useRouter();
const isLoading = ref(true);
const productData = ref({});
const mainImage = ref("");
const quantity = ref(0);

// Method Declaration
// Method to fetch the product detail from API
const fetchProduct = async () => {
  try {
    const response = await axios.get(
      "/client/products/fall-limited-edition-sneakers"
    );
    productData.value = response.data.data;
    mainImage.value = productData.value.images[0];
  } catch (error) {
    if (error.response && error.response.status === 404) {
      router.push({ name: "NotFound" });
    } else {
      console.error("Error fetching product:", error);
    }
  } finally {
    isLoading.value = false;
  }
};
// Method to update the image link
const updateImageLink = (index) => {
  mainImage.value = productData.value.images[index];
};

// Method to increment the Quantity
const incrementQuantity = () => {
  quantity.value += 1;
};

// Method to decrement the Quantity
const decrementQuantity = () => {
  if (quantity.value > 1) {
    quantity.value -= 1;
  }
};

onMounted(() => {
  fetchProduct();
});
</script>

