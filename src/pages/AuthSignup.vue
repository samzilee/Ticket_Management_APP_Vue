<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
const router = useRouter()
const email = ref('')
const password = ref('')
const confirm = ref('')
const error = ref('')
const loading = ref(false)

const submit = (e) => {
  e.preventDefault()
  error.value = ''
  if (!email.value || !password.value || !confirm.value) { error.value = 'Please fill in all fields.'; return }
  if (password.value !== confirm.value) { error.value = 'Passwords do not match.'; return }
  loading.value = true
  setTimeout(() => {
    loading.value = false
    localStorage.setItem('ticketapp_session', JSON.stringify({ email: email.value }))
    router.push('/dashboard')
  }, 800)
}
</script>

<template>
  <div class="auth-root">
    <form class="auth-form" @submit="submit">
      <h2>Sign Up</h2>
      <label for="email">Email</label>
      <input id="email" type="email" v-model="email" autocomplete="username"  />
      <label for="password">Password</label>
      <input id="password" type="password" v-model="password" autocomplete="new-password"  />
      <label for="confirm">Confirm Password</label>
      <input id="confirm" type="password" v-model="confirm" autocomplete="new-password"  />
      <div v-if="error" class="auth-error" role="alert">{{ error }}</div>
      <button class="auth-btn" type="submit" :disabled="loading">{{ loading ? 'Signing up...' : 'Sign Up' }}</button>
      <div class="auth-alt">
        <span>Already have an account?</span>
        <button type="button" class="auth-link" @click.prevent="router.push('/auth/login')">Login</button>
      </div>
    </form>
    <!-- Toast notification placeholder -->
  </div>
</template>
