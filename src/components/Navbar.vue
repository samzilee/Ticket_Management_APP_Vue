<script setup>
import { useRouter, useRoute } from 'vue-router'
import { ref, watch } from 'vue'
const router = useRouter()
const route = useRoute()
const session = ref(typeof window !== 'undefined' ? localStorage.getItem('ticketapp_session') : null)

const go = (p) => router.push(p)

watch(route, () => {
  session.value = typeof window !== 'undefined' ? localStorage.getItem('ticketapp_session') : null
})
</script>

<template>
  <nav class="navbar-root" aria-label="Main navigation">
    <div class="navbar-content">
      <button class="navbar-logo" @click="go('/')">TicketFlow</button>
      <div class="navbar-links">
        <button :class="{ active: route.path === '/' }" @click="go('/')">Home</button>
        <button :class="{ active: route.path === '/dashboard' }" @click="go('/dashboard')">Dashboard</button>
          <button :class="{ active: route.path === '/tickets' }" @click="go('/tickets')">Tickets</button>
       
      </div>
    </div>
  </nav>
</template>
