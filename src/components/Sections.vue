<script setup>
import { defineAsyncComponent } from 'vue';

const props = defineProps({ items: Array });

// Template-literal dynamic import — Rollup expands this to a glob and
// emits a chunk per matching file. This is the pattern that exposes
// the Astro 6 hybrid build hash-reconciliation gap: each chunk's body
// contains an `import("/_astro/<name>.<hash>.js")` URL whose hash is
// computed against a different Vite pass's hash table than the one
// used to write the actual chunk filenames.
function load(name) {
  return defineAsyncComponent(() => import(`../sections/${name}.vue`));
}
</script>

<template>
  <div>
    <component
      v-for="(item, i) in items"
      :key="i"
      :is="load(item.name)"
      :data="item.data"
    />
  </div>
</template>
