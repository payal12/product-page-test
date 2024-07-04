<template>
    <div class="product-page">
        <div class="image-gallery">
            <img :src="mainImage" alt="Main Product Image" class="main-image" />
            <div class="sub-images">
                <img
                    v-for="(image, index) in productData.images"
                    :key="index"
                    :src="image"
                    alt="Sub Image"
                    class="sub-image"
                />
        </div>
      </div>
      <div class="product-details">
        <h1>{{ productData.name }}</h1>
        <p>{{ productData.description }}</p>
      </div>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

const productData = ref({})
const mainImage = ref('')

// Method to fetch the product detail from API
const fetchProduct = async() => {
    try {
    const response = await axios.get('/client/products/fall-limited-edition-sneakers');
    productData.value = response.data.data;
    console.log("product data",productData.value)
    mainImage.value = productData.value.images[0];
    console.log("mainImage",mainImage.value)
  } catch (error) {
    if (error.response && error.response.status === 404) {
      console.error('Not found')
    } else {
      console.error('Error fetching product:', error);
    }
  }
}
onMounted(()=> {
    fetchProduct();
})
</script>
<style scoped>
.product-page {
    display: flex;
    padding: 20px;
}
.image-gallery {
  flex: 1;
}
.main-image {
  width: 100%;
  max-width: 500px;
  height: auto;
  margin-bottom: 20px;
  border-radius: 10px;
}
.sub-images {
  display: flex;
  gap: 10px;
}
.sub-image {
  width: 100px;
  height: 100px;
  cursor: pointer;
  border: 2px solid transparent;
  border-radius: 10px;
}

.product-details {
  flex: 1;
  padding: 20px;
}


</style>