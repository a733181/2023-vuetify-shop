<template lang="pug">
div
  v-row
    v-col(cols="12")
      h1.text-center 註冊
      v-divider
    v-col(cols="12")
      v-form(v-model="valid" @submit.prevent="register")
        v-text-field(label="信箱" type="email" v-model="form.email" :rules="[rules.email,rules.required]")
        v-text-field(label="帳號" type="text" v-model="form.account" :rules="[rules.length,rules.required]" counter="20" maxlength="20")
        v-text-field(label="密碼" type="password" v-model="form.password" :rules="[rules.length,rules.required]" counter="20" maxlength="20")
        v-text-field(label="確認密碼" type="password" v-model="form.passwordConfirm" :rules="[rules.length,rules.required,rules.passwordConfirm]" counter="20" maxlength="20")
        .text-center.mt-5
          v-btn(color="success" type="submit" size="large" :loading="loading") 註冊
</template>

<script setup>
import { ref, reactive } from 'vue';
import validator from 'validator';
import Swal from 'sweetalert2';
import { useRouter } from 'vue-router';
import { api } from '@/plugins/axios';

const router = useRouter();
const valid = ref(false);
const loading = ref(false);
const form = reactive({
  account: '',
  password: '',
  passwordConfirm: '',
  email: '',
});
const rules = {
  email(value) {
    return validator.isEmail(value) || '格式錯誤';
  },
  required(value) {
    return !!value || '欄位必填';
  },
  length(value) {
    return value.length >= 4 || value.length <= 20 || '必須 4~20 個字';
  },
  passwordConfirm(value) {
    return value === form.password || '密碼不一致';
  },
};

const register = async () => {
  if (!valid.value) return;
  loading.value = true;
  try {
    await api.post('/users', form);
    await Swal.fire({
      icon: 'success',
      title: '成功',
      text: '註冊成功',
    });
    router.push('/');
  } catch (error) {
    Swal.fire({
      icon: 'error',
      title: '失敗',
      text: error?.response?.data?.message || '發生錯誤',
    });
  }
  loading.value = false;
};
</script>
