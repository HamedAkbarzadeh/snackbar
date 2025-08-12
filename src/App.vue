<template>
  <div class="w-full h-screen flex flex-col justify-center items-center gap-5">
    <h1>title</h1>
    <button @click="snackbarRef.handleOpen()" class="text-red-500 border-2 p-2">opne this</button>

    <Snackbar dir="rtl" type="info" position="top" :done="true" ref="snackbarRef">
      <template #message>
        عملیات با موفقیت انجام شد.
      </template>
    </Snackbar>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import Snackbar from './components/Snackbar.vue';

const snackbarRef = ref(null);



</script>

<style lang="scss" scoped></style>