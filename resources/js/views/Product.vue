<template>
  <div class="product-page">
    <div class="product-section">
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
            <button @click="decrementQuantity">-</button>
            <div>{{ quantity }}</div>
            <button @click="incrementQuantity">+</button>
          </div>
          <button class="add-to-cart d-flex">
            <span class="cart"></span><span>Add to cart</span>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { useRoute, useRouter } from 'vue-router';
import axios from "axios";


const route = useRoute();
const router = useRouter();
const productData = ref({});
const mainImage = ref("");
const quantity = ref(0);
// Method to fetch the product detail from API
const fetchProduct = async () => {
  try {
    const response = await axios.get(
      "/client/products/fall-limited-edition-sneakers"
    );
    productData.value = response.data.data;
    console.log("product data", productData.value);
    mainImage.value = productData.value.images[0];
    console.log("mainImage", mainImage.value);
  } catch (error) {
    if (error.response && error.response.status === 404) {
      router.push({ name: 'NotFound' });
    } else {
      console.error("Error fetching product:", error);
    }
  }
};
const updateImageLink = (index) => {
  mainImage.value = productData.value.images[index];
};

const incrementQuantity = () => {
  quantity.value += 1;
};

const decrementQuantity = () => {
  if (quantity.value > 1) {
    quantity.value -= 1;
  }
};

onMounted(() => {
  fetchProduct();
});
</script>

<style scoped>
.product-page {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}

.product-section {
  display: flex;
  width: 100%;
  margin: 25px;
}

.product-section > div {
  width: 50%;
}

.image-gallery {
  justify-content: center;
  align-items: center;
  display: flex;
  flex-direction: column;
  gap: 30px;
}

.main-image {
  width: 24rem;
  height: 24rem;
  border-radius: 10px;
}

.sub-images {
  display: flex;
  gap: 20px;
}

.sub-image {
  width: 5rem;
  height: 5rem;
  border-radius: 10px;
}

.sub-image.selected {
  box-shadow: 0px 0px 0px 2px rgb(172 76 2);
  opacity: 0.3;
}

.product-details h1 {
  font-size: 36px;
}

.product-details p {
  font-size: 16px;
  color: hsl(220, 14%, 75%);
}

.product-details .product-discounted {
  font-weight: bold;
  font-size: 22px;
  margin-right: 10px;
}

.product-details .discount-badge {
  background: hsl(25, 100%, 94%);
  color: hsl(26, 100%, 55%);
  font-size: 14px;
  font-weight: 600;
  padding: 1px 7px;
  border-radius: 5px;
}

.product-details .full-price {
  text-decoration: line-through;
  font-size: 14px;
  color: #ccc;
  margin-block: 15px;
  font-weight: 600;
}

.d-flex {
  display: flex;
}

.quantity-selector {
  display: flex;
  background: #f7f8fd;
  gap: 17px;
  padding: 10px;
  align-items: center;
  font-weight: 500;
  border-radius: 10px;
}

.quantity-selector div {
  width: 20px;
  font-weight: bold;
}

.quantity-selector button {
  border: 0;
  background: transparent;
  color: hsl(26, 100%, 55%);
  padding-inline: 10px;
  font-size: 22px;
  font-weight: bold;
}

.add-to-cart {
  background-color: hsl(26, 100%, 55%);
  color: #fff;
  border-radius: 10px;
  padding: 10px 50px;
  margin-left: 15px;
  font-size: 16px;
  border: none;
  gap: 12px;
  align-items: center;
}

.cart {
  background-image: url(/images/cart.png);
  width: 20px;
  display: block;
  height: 20px;
  background-size: contain;
  background-repeat: no-repeat;
}

@media (min-width: 375px) and (max-width: 760px) {
  .product-page .product-section {
    flex-direction: column;
    margin: 10px;
  }

  .product-page .product-section > div {
    width: 100%;
  }

  .product-page .product-section .image-gallery {
    gap: 10px;
  }
}
</style>
