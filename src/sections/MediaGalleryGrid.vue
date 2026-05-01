<script setup>
import { onMounted, ref } from 'vue';

defineProps({ data: Object });

const root = ref(null);

onMounted(async () => {
  if (typeof window === 'undefined') return;
  const { default: GLightbox } = await import('glightbox');
  GLightbox({ selector: '.media-gallery-lightbox' });
});
</script>

<template>
  <div ref="root" class="media-gallery-grid">
    <a href="https://picsum.photos/1200/800?random=30" class="media-gallery-lightbox">
      <img src="https://picsum.photos/300/200?random=30" alt="" />
    </a>
    <a href="https://picsum.photos/1200/800?random=31" class="media-gallery-lightbox">
      <img src="https://picsum.photos/300/200?random=31" alt="" />
    </a>
    <a href="https://picsum.photos/1200/800?random=32" class="media-gallery-lightbox">
      <img src="https://picsum.photos/300/200?random=32" alt="" />
    </a>
  </div>
</template>
