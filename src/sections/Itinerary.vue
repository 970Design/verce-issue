<script setup>
import { onMounted, ref } from 'vue';
import 'glightbox/dist/css/glightbox.min.css';

defineProps({ fields: Object, index: Number });

const root = ref(null);
const status = ref('Loading lightbox...');

onMounted(async () => {
  if (typeof window === 'undefined') return;
  const { default: GLightbox } = await import('glightbox');
  GLightbox({ selector: '.itinerary-lightbox' });
  status.value = 'Lightbox ready';
});
</script>

<template>
  <div ref="root" class="itinerary">
    <h2>Itinerary <small>— status: {{ status }}</small></h2>
    <ol>
      <li>
        Day 1
        <a href="https://picsum.photos/1000/700?random=20" class="itinerary-lightbox" target="_blank" rel="noopener">
          <img src="https://picsum.photos/200/120?random=20" alt="" />
        </a>
      </li>
      <li>Day 2</li>
      <li>Day 3</li>
    </ol>
  </div>
</template>
