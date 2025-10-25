<script setup>
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import Toast from '../components/Toast.vue'
const router = useRouter()
const tickets = ref([])
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

const total = () => tickets.value.length
const open = () => tickets.value.filter(t => t.status === 'open').length
const resolved = () => tickets.value.filter(t => t.status === 'closed').length

const handleLogout = () => { localStorage.removeItem('ticketapp_session'); router.push('/') }
const closeToast = () => toast.value = null
</script>

<template>
  <div class="dashboard-root">
    <header class="dashboard-header">
      <h1>Dashboard</h1>
      <button class="dashboard-logout" @click="handleLogout">Logout</button>
    </header>
    <section class="dashboard-cards">
      <div class="dashboard-card">
        <span class="dashboard-label">Total Tickets</span>
        <span class="dashboard-value">{{ total() }}</span>
      </div>
      <div class="dashboard-card status-open">
        <span class="dashboard-label">Open Tickets</span>
        <span class="dashboard-value">{{ open() }}</span>
      </div>
      <div class="dashboard-card status-closed">
        <span class="dashboard-label">Resolved Tickets</span>
        <span class="dashboard-value">{{ resolved() }}</span>
      </div>
    </section>
    <nav class="dashboard-nav">
      <button class="dashboard-nav-btn" @click="router.push('/tickets')">Go to Ticket Management</button>
    </nav>
    <Toast v-if="toast" :message="toast.message" :type="toast.type" @remove="closeToast" />
  </div>
</template>
