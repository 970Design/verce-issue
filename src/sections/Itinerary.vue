<script setup>
import { onMounted, ref } from 'vue';

defineProps({ data: Object });

const root = ref(null);

onMounted(async () => {
  if (typeof window === 'undefined') return;
  const { default: GLightbox } = await import('glightbox');
  GLightbox({ selector: '.itinerary-lightbox' });
});
</script>

<template>
  <div ref="root" class="itinerary">
    <ol>
      <li>
        Day 1
        <a href="https://picsum.photos/1000/700?random=20" class="itinerary-lightbox">
          <img src="https://picsum.photos/200/120?random=20" alt="" />
        </a>
      </li>
      <li>Day 2</li>
      <li>Day 3</li>
    </ol>
  </div>
</template>
