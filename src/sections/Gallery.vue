<script setup>
import { onMounted, ref } from 'vue';
import 'glightbox/dist/css/glightbox.min.css';

defineProps({ fields: Object, index: Number });

const galleryRef = ref(null);

onMounted(async () => {
  if (typeof window === 'undefined' || !galleryRef.value) return;
  const { default: GLightbox } = await import('glightbox');
  GLightbox({ selector: '.glightbox' });
});
</script>

<template>
  <div ref="galleryRef" class="gallery">
    <a href="https://picsum.photos/1200/800?random=1" class="glightbox">
      <img src="https://picsum.photos/300/200?random=1" alt="" />
    </a>
    <a href="https://picsum.photos/1200/800?random=2" class="glightbox">
      <img src="https://picsum.photos/300/200?random=2" alt="" />
    </a>
  </div>
</template>
