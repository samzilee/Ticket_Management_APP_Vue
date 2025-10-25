<script setup>
import { ref, watch } from 'vue'
const props = defineProps({ ticket: { type: Object, default: null } })
const emits = defineEmits(['save', 'cancel'])

const title = ref('')
const status = ref('open')
const description = ref('')
const error = ref('')

watch(() => props.ticket, (t) => {
  if (t) {
    title.value = t.title || ''
    status.value = t.status || 'open'
    description.value = t.description || ''
  } else {
    title.value = ''
    status.value = 'open'
    description.value = ''
  }
  error.value = ''
}, { immediate: true })

const submit = (e) => {
  e.preventDefault()
  if (!title.value.trim()) { error.value = 'Title is required.'; return }
  if (!['open','in_progress','closed'].includes(status.value)) { error.value = 'Status must be valid.'; return }
  error.value = ''
  if (props.ticket && props.ticket.id) {
    emits('save', { ...props.ticket, title: title.value, status: status.value, description: description.value })
  } else {
    emits('save', { title: title.value, status: status.value, description: description.value })
  }
}

</script>

<template>
  <form class="ticket-form" @submit="submit">
    <h2>{{ props.ticket ? 'Edit Ticket' : 'New Ticket' }}</h2>
    <label for="title">Title</label>
    <input id="title" v-model="title" />
    <label for="status">Status</label>
    <select id="status" v-model="status" required>
      <option value="open">Open</option>
      <option value="in_progress">In Progress</option>
      <option value="closed">Closed</option>
    </select>
    <label for="desc">Description</label>
    <textarea id="desc" v-model="description" rows="3"></textarea>
    <div v-if="error" class="form-error" role="alert">{{ error }}</div>
    <div class="form-actions">
      <button class="form-save" type="submit">{{ props.ticket ? 'Update' : 'Create' }}</button>
      <button class="form-cancel" type="button" @click.prevent="emits('cancel')">Cancel</button>
    </div>
  </form>
</template>
