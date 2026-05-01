<script setup>
import { defineAsyncComponent, onErrorCaptured } from 'vue';

const props = defineProps(['component', 'fields', 'index']);

// Template-literal dynamic import — Rollup expands this to a glob and
// emits a chunk per matching file. This is the dispatcher pattern that
// exposes the Astro 6 hybrid build hash-reconciliation gap when the
// component is rendered server-side AND client-side (`client:load`):
// the Server/Prerender pass and the Client pass each compute their own
// hash table, and the URLs baked into chunk bodies can reference hashes
// that the Client pass never emitted.
const Section = defineAsyncComponent(() => import(`../sections/${props.component}.vue`));

onErrorCaptured(() => false);
</script>

<template>
  <Section :fields="fields" :index="index" />
</template>
