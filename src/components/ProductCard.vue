<template lang="pug">
v-card
  v-img(:src="image" cover height="200")
  v-card-title
    RouterLink.text-decoration-none.text-primary(:to="'/products/'+_id") {{ name }}
  v-card-subtitle ${{ price }}
  v-card-text.pre {{ description }}
  v-card-actions
    v-btn(color="primary" prepend-icon="mdi-cart" @click="editCart({ _id , quantity:1 })") 加入購物車
</template>

<script setup>
import { useUserStore } from '@/stores/user';

defineProps({
  _id: {
    type: String,
    default: '',
  },
  name: {
    type: String,
    default: '',
  },
  price: {
    type: Number,
    default: 0,
  },
  description: {
    type: String,
    default: '',
  },
  image: {
    type: String,
    default: '',
  },
  sell: {
    type: Boolean,
    default: false,
  },
  category: {
    type: String,
    default: '',
  },
});

const user = useUserStore();
const { editCart } = user;
</script>
