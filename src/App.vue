<script setup>
import { ref, watch } from 'vue'
import NovaRestaurant from './NovaRestaurant.vue'

const language = ref(localStorage.getItem('lang') || 'ar')

function changeLanguage() {
  language.value = language.value === 'ar' ? 'fr' : 'ar'
}

watch(language, (val) => {
  localStorage.setItem('lang', val)
  document.documentElement.lang = val
  document.documentElement.dir = val === 'ar' ? 'rtl' : 'ltr'
}, { immediate: true })
</script>

<template>
  <NovaRestaurant :language="language" @change-language="changeLanguage" />
</template>