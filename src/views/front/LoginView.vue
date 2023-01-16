<template lang="pug">
div
  v-row
    v-col(cols="12")
      h1.text-center 登入
      v-divider
    v-col(cols="12")
      v-form(@submit.prevent="login")
        v-text-field(label="帳號" type="text" v-model="form.account" :rules="[rules.length,rules.required]" counter="20" maxlength="20")
        v-text-field(label="密碼" type="password" v-model="form.password" :rules="[rules.length,rules.required]" counter="20" maxlength="20")
        .text-center.mt-5
          v-btn(color="success" type="submit" size="large" :loading="loading") 登入
  </template>
<script setup>
import { ref, reactive } from 'vue';
import { useUserStore } from '@/stores/user';

const user = useUserStore();

const loading = ref(false);

const form = reactive({
  account: '',
  password: '',
});
const rules = {
  required(value) {
    return !!value || '欄位必填';
  },
  length(value) {
    return value.length >= 4 || value.length <= 20 || '必須 4~20 個字';
  },
};

const login = async () => {
  loading.value = true;
  await user.login(form);
  loading.value = false;
};
</script>
