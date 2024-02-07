<script setup>
import { onMounted, ref } from 'vue'
import useAPI from '@/composables/useAPI';
import BaseTitle from '@/components/BaseTitle.vue';

const {categories, getCategories} = useAPI()

onMounted(async () => {
  await getCategories()
})

</script>

<template>
  <BaseTitle>TRIVIA APPLICATION</BaseTitle>

  <div v-for="category in categories" key="category.id">
    {{ category.name }}
  </div>
</template>
