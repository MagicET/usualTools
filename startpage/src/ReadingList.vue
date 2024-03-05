<script setup>
  import { ref } from 'vue'
  const props = defineProps({
    url: String
  })
  const emit = defineEmits(["delete"])
</script>

<template>
  <div>
    <a :href="url">{{ url }}</a>
    <button @click="$emit('delete')"><span class="material-symbols-outlined">
delete
</span></button>
  </div>
</template>
