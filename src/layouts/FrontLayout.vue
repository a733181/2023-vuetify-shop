<template lang="pug">
v-app-bar(color="primary")
  v-app-bar-title
    RouterLink.text-decoration-none.text-white(to="/") 購物網
  v-spacer
  v-btn(v-if="!isLogin" prepend-icon="mdi-account-plus" to="/register" variant="text") 註冊
  v-btn(v-if="!isLogin" prepend-icon="mdi-login" to="/login" variant="text") 登入
  v-btn(v-if="isLogin" prepend-icon="mdi-cart" variant="text" to="/cart")
    v-badge(:content="cart" color="success" floating) 購物車
  v-btn(v-if="isLogin"  prepend-icon="mdi-format-list-bulleted" to="/orders" variant="text") 訂單
  v-btn(v-if="isLogin && isAdmin"  prepend-icon="mdi-cog" to="/admin" variant="text") 管理
  v-btn(v-if="isLogin" prepend-icon="mdi-logout" variant="text" @click="logout") 登出
v-main
  v-container
    RouterView
</template>

<script setup>
import { storeToRefs } from 'pinia';
import { useUserStore } from '@/stores/user';

const user = useUserStore();
const { isLogin, isAdmin, cart } = storeToRefs(user);
const { logout } = user;
</script>
