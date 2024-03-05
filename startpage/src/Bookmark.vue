<script setup>
  import { ref, computed } from 'vue'
  const props = defineProps({
    name: String,
    url: String, 
    icon: String
  })

  const computedUrl = computed(() => {
    if(props.url.includes("http")) {
      return props.url
    } else {
      return "//" + props.url
    }
  });

  const emit = defineEmits(["delete", "update"])
</script>

<template>
  <div>
    <a :href="computedUrl">
      <svg viewBox="0 0 100 100" class="bookmarkIcon">
        <text x="50%" y="55%" dominant-baseline="middle" text-anchor="middle" font-size="60">
          {{ icon }}
        </text>
      </svg>
    </a>
    <div class="bookmarkNameBar">
      <div class="bookmarkName">{{ name }}</div>
      <button @click="$emit('update')">
        <span class="material-symbols-outlined">more_vert</span>
      </button>
    </div>
  </div>
</template>