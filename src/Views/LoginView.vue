<template>
  <div
    class="min-h-screen w-full bg-gradient-to-br from-primary to-secondary flex items-center justify-center"
  >
    <div class="card w-96 bg-base-100 shadow-xl">
      <div class="card-body">
        <h2 class="card-title text-2xl font-bold text-center mb-6 justify-center">Login</h2>
        <form @submit.prevent="handleLogin">
          <div class="form-control w-full max-w-xs">
            <label class="label">
              <span class="label-text">Nickname</span>
            </label>
            <input
              type="text"
              v-model="nickname"
              placeholder="Type here"
              class="input input-bordered w-full max-w-xs"
              required
            />
          </div>
          <div class="form-control w-full max-w-xs">
            <label class="label">
              <span class="label-text">Password</span>
            </label>
            <input
              type="password"
              v-model="password"
              placeholder="Enter your password"
              class="input input-bordered w-full max-w-xs"
              required
            />
          </div>
          <div class="form-control mt-6">
            <button class="btn btn-primary" :class="{ loading: isLoading }">
              {{ isLoading ? 'Loading...' : 'Login' }}
            </button>
          </div>
          <div v-if="error" class="alert alert-error mt-4">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="stroke-current shrink-0 h-6 w-6"
              fill="none"
              viewBox="0 0 24 24"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M10 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2m7-2a9 9 0 11-18 0 9 9 0 0118 0z"
              />
            </svg>
            <span>{{ error }}</span>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'LoginView',
  data() {
    return {
      nickname: '',
      password: '',
      error: null,
      isLoading: false,
    }
  },
  methods: {
    async handleLogin() {
      try {
        this.isLoading = true
        this.error = null
        const response = await axios.post('/api/login', {
          nickname: this.nickname,
          password: this.password,
        })
        localStorage.setItem('token', response.data.token) // Save token to local storage

        this.$router.push('/dashboard') // Redirect to dashboard
      } catch (err) {
        this.error = err.response?.data?.message || 'Login failed'
      } finally {
        this.isLoading = false
      }
    },
  },
}
</script>
