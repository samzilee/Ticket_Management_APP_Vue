<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  toasts: { type: Array, default: null },
  message: { type: String, default: '' },
  type: { type: String, default: 'success' }
})
const emit = defineEmits(['remove'])

const localVisible = ref(true)
let timer = null

watch(() => props.message, (m) => {
  if (m) {
    localVisible.value = true
    if (timer) clearTimeout(timer)
    timer = setTimeout(() => {
      localVisible.value = false
      emit('remove')
    }, 2500)
  }
})

const remove = (id) => emit('remove', id)
</script>

<template>
  <div>
    <template v-if="Array.isArray(toasts) && toasts.length">
      <div v-for="t in toasts" :key="t.id" :class="['toast', t.type]" role="status">
        {{ t.message }}
        <button class="toast-close" @click="remove(t.id)" aria-label="Close notification">×</button>
      </div>
    </template>
    <template v-else-if="message">
      <div v-if="localVisible" :class="['toast', type]" role="status">
        {{ message }}
        <button class="toast-close" @click="remove()" aria-label="Close notification">×</button>
      </div>
    </template>
  </div>
</template>
