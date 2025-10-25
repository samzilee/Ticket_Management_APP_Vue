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

<style scoped>
.ticket-form {
  background: #fff;
  border-radius: 1.25rem;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.07);
  padding: 2rem 1.5rem;
  max-width: 400px;
  width: 100%;
  margin: 1.5rem auto 2rem auto;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}
.ticket-form h2 {
  color: #3730a3;
  margin-bottom: 0.5rem;
  font-size: 1.4rem;
  text-align: center;
}
.ticket-form label {
  font-weight: 600;
  color: #6366f1;
  margin-bottom: 0.2rem;
}
.ticket-form input,
.ticket-form select,
.ticket-form textarea {
  padding: 0.7em 1em;
  border-radius: 0.7em;
  border: 1px solid #c7d2fe;
  font-size: 1rem;
  outline: none;
  transition: border 0.2s;
}
.ticket-form input:focus,
.ticket-form select:focus,
.ticket-form textarea:focus {
  border: 1.5px solid #6366f1;
}
.form-error {
  color: #b91c1c;
  background: #fee2e2;
  border-radius: 0.5em;
  padding: 0.5em 1em;
  font-size: 0.98rem;
  margin-bottom: 0.5rem;
  text-align: left;
}
.form-actions {
  display: flex;
  gap: 1em;
  justify-content: flex-end;
}
.form-save {
  background: #6366f1;
  color: #fff;
  border: none;
  border-radius: 2em;
  padding: 0.7em 2em;
  font-weight: 700;
  font-size: 1rem;
  cursor: pointer;
  transition: background 0.2s;
}
.form-save:hover {
  background: #3730a3;
}
.form-cancel {
  background: #e0e7ff;
  color: #3730a3;
  border: none;
  border-radius: 2em;
  padding: 0.7em 2em;
  font-weight: 600;
  font-size: 1rem;
  cursor: pointer;
  transition: background 0.2s;
}
.form-cancel:hover {
  background: #6366f1;
  color: #fff;
}
</style>
