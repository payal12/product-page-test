<template>
    <div>product page here</div>
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