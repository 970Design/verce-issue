<script setup>
import { onMounted, ref } from 'vue';

defineProps({ data: Object });

const root = ref(null);

onMounted(async () => {
  if (typeof window === 'undefined') return;
  const { default: GLightbox } = await import('glightbox');
  GLightbox({ selector: '.bio-card-lightbox' });
});
</script>

<template>
  <div ref="root" class="bio-card-grid">
    <div class="card">
      <a href="https://picsum.photos/800/800?random=10" class="bio-card-lightbox">
        <img src="https://picsum.photos/200/200?random=10" alt="" />
      </a>
      <h3>Bio Card</h3>
    </div>
  </div>
</template>
