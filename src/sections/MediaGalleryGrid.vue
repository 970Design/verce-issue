<script setup>
import { onMounted, ref } from 'vue';
import 'glightbox/dist/css/glightbox.min.css';
import Hls from 'hls.js';

defineProps({ fields: Object, index: Number });

const root = ref(null);
const status = ref('Loading lightbox...');

onMounted(async () => {
  if (typeof window === 'undefined') return;
  const { default: GLightbox } = await import('glightbox');
  GLightbox({ selector: '.media-gallery-lightbox' });
  if (Hls.isSupported && Hls.isSupported()) { /* noop */ }
  status.value = 'Lightbox ready';
});
</script>

<template>
  <div ref="root" class="media-gallery-grid">
    <h2>MediaGalleryGrid <small>— status: {{ status }}</small></h2>
    <a href="https://picsum.photos/1200/800?random=30" class="media-gallery-lightbox" target="_blank" rel="noopener">
      <img src="https://picsum.photos/300/200?random=30" alt="" />
    </a>
    <a href="https://picsum.photos/1200/800?random=31" class="media-gallery-lightbox" target="_blank" rel="noopener">
      <img src="https://picsum.photos/300/200?random=31" alt="" />
    </a>
    <a href="https://picsum.photos/1200/800?random=32" class="media-gallery-lightbox" target="_blank" rel="noopener">
      <img src="https://picsum.photos/300/200?random=32" alt="" />
    </a>
  </div>
</template>
