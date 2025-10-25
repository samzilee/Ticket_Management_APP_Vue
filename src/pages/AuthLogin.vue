<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
const router = useRouter()
const email = ref('')
const password = ref('')
const error = ref('')
const loading = ref(false)

const submit = (e) => {
  e.preventDefault()
  error.value = ''
  if (!email.value || !password.value) { error.value = 'Please fill in all fields.'; return }
  loading.value = true
  setTimeout(() => {
    loading.value = false
    if (email.value === 'demo@ticketflow.com' && password.value === 'demo123') {
      localStorage.setItem('ticketapp_session', JSON.stringify({ email: email.value }))
      router.push('/dashboard')
    } else {
      error.value = 'Invalid credentials. Try demo@ticketflow.com / demo123'
    }
  }, 800)
}
</script>

<template>
  <div class="auth-root">
    <form class="auth-form" @submit="submit">
      <h2>Login</h2>
      <label for="email">Email</label>
      <input id="email" type="email" v-model="email" autocomplete="username" />
      <label for="password">Password</label>
      <input id="password" type="password" v-model="password" autocomplete="current-password"  />
      <div v-if="error" class="auth-error" role="alert">{{ error }}</div>
      <button class="auth-btn" type="submit" :disabled="loading">{{ loading ? 'Logging in...' : 'Login' }}</button>
      <div class="auth-alt">
        <span>Don't have an account?</span>
        <button type="button" class="auth-link" @click.prevent="router.push('/auth/signup')">Sign Up</button>
      </div>
    </form>
    <!-- Toast notification placeholder -->
  </div>
</template>
