<script setup>
import { onMounted, ref } from 'vue';
import 'glightbox/dist/css/glightbox.min.css';

defineProps({ fields: Object, index: Number });

const root = ref(null);
const status = ref('Loading lightbox...');

onMounted(async () => {
  if (typeof window === 'undefined') return;
  const { default: GLightbox } = await import('glightbox');
  GLightbox({ selector: '.bio-card-lightbox' });
  status.value = 'Lightbox ready';
});
</script>

<template>
  <div ref="root" class="bio-card-grid">
    <h2>BioCardGrid <small>— status: {{ status }}</small></h2>
    <div class="card">
      <a href="https://picsum.photos/800/800?random=10" class="bio-card-lightbox" target="_blank" rel="noopener">
        <img src="https://picsum.photos/200/200?random=10" alt="" />
      </a>
      <h3>Bio Card</h3>
    </div>
  </div>
</template>
