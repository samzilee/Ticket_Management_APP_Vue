<script setup>
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import TicketCard from '../components/TicketCard.vue'
import Form from '../components/Form.vue'
import Toast from '../components/Toast.vue'

const router = useRouter()
const tickets = ref([])
const editing = ref(null)
const showForm = ref(false)
const toast = ref(null)

onMounted(() => {
  const session = localStorage.getItem('ticketapp_session')
  if (!session) {
    toast.value = { message: 'Your session has expired — please log in again.', type: 'error' }
    setTimeout(() => router.push('/auth/login'), 1200)
    return
  }
  try {
    const stored = localStorage.getItem('ticketapp_tickets')
    tickets.value = stored ? JSON.parse(stored) : []
  } catch (e) {
    toast.value = { message: 'Failed to load tickets. Please retry.', type: 'error' }
  }
})

const saveTickets = (newTickets) => { tickets.value = newTickets; localStorage.setItem('ticketapp_tickets', JSON.stringify(newTickets)) }

const handleCreate = (ticket) => {
  if (!ticket.title.trim()) { toast.value = { message: 'Title is required.', type: 'error' }; return }
  if (!['open','in_progress','closed'].includes(ticket.status)) { toast.value = { message: 'Status must be valid.', type: 'error' }; return }
  const newTicket = { ...ticket, id: Date.now().toString() }
  const newTickets = [newTicket, ...tickets.value]
  saveTickets(newTickets)
  toast.value = { message: 'Ticket created!', type: 'success' }
  showForm.value = false
}

const handleUpdate = (ticket) => {
  if (!ticket.title.trim()) { toast.value = { message: 'Title is required.', type: 'error' }; return }
  if (!['open','in_progress','closed'].includes(ticket.status)) { toast.value = { message: 'Status must be valid.', type: 'error' }; return }
  const newTickets = tickets.value.map(t => t.id === ticket.id ? ticket : t)
  saveTickets(newTickets)
  toast.value = { message: 'Ticket updated.', type: 'success' }
  editing.value = null
  showForm.value = false
}

const handleDelete = (id) => {
  if (!confirm('Delete this ticket?')) return
  const newTickets = tickets.value.filter(t => t.id !== id)
  saveTickets(newTickets)
  toast.value = { message: 'Ticket deleted.', type: 'success' }
}

const handleEdit = (ticket) => { editing.value = ticket; showForm.value = true }
const closeToast = () => toast.value = null
</script>

<template>
  <div class="tickets-root">
    <header class="tickets-header">
      <h1>Ticket Management</h1>
      <button class="tickets-create-btn" @click="() => { editing = null; showForm = true }" @click.prevent="(()=>{ editing=null; showForm=true })()">+ New Ticket</button>
    </header>
    <Form v-if="showForm" :ticket="editing" @save="(ticket) => (editing ? handleUpdate(ticket) : handleCreate(ticket))" @cancel="() => { editing=null; showForm=false }" />
    <section class="tickets-list">
      <div v-if="tickets.length === 0" class="tickets-empty">No tickets yet.</div>
      <div v-else class="grid">
        <TicketCard v-for="t in tickets" :key="t.id" :ticket="t" @edit="() => handleEdit(t)" @delete="() => handleDelete(t.id)" />
      </div>
    </section>
    <Toast v-if="toast" :message="toast.message" :type="toast.type" @remove="closeToast" />
  </div>
</template>
