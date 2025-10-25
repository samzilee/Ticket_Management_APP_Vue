<script setup>
const props = defineProps({
  ticket: { type: Object, required: true },
})
const emits = defineEmits(['edit', 'delete'])

const doEdit = () => emits('edit')
const doDelete = () => {
  if (confirm('Delete this ticket?')) emits('delete')
}
</script>

<template>
  <div :class="['ticket-card', `status-${ticket.status}`]">
    <div class="ticket-card-header">
      <h3>{{ ticket.title }}</h3>
      <span :class="['ticket-status', `status-${ticket.status}`]">{{ ticket.status.replace('_', ' ') }}</span>
    </div>
    <p v-if="ticket.description" class="ticket-desc">{{ ticket.description }}</p>
    <div class="ticket-card-actions">
      <button class="ticket-edit" @click="doEdit">Edit</button>
      <button class="ticket-delete" @click="doDelete">Delete</button>
    </div>
  </div>
</template>
