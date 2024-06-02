<script setup>
useHead({
  htmlAttrs: { lang: 'zh-Hans' },
  title: 'Fancy',
  link: [{ rel: 'icon', href: '/favicon.ico' }],
  meta: [
    { name: 'viewport', content: 'width=device-width, initial-scale=1' },
    { name: 'description', content: 'Home page description' }
  ]
})
</script>

<template>
  <AppHeader />
  <NuxtPage />
</template>
