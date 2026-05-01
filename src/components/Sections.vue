<script setup>
import Section from './Section.vue';

defineProps({ items: Array });
</script>

<template>
  <Section
    v-for="(item, i) in items"
    :key="i"
    :component="item.name"
    :fields="item.data"
    :index="i"
  />
</template>
