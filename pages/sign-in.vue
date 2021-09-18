<template>
  <div class="sign-in">
    <p>sign-in</p>
    <input v-model="email" type="email" placeholder="Email" />
    <input v-model="password" type="password" placeholder="Password" />
    <button type="button" @click="onClick">Submit</button>
  </div>
</template>

<script>
export default {
  data () {
    return {
      email: '',
      password: '',
    }
  },
  methods: {
    async onClick () {
      const url = 'http://localhost:4500/api/admin/sign-in'

      const body = {
        email: this.email,
        password: this.password,
      }

      try {
        const res = await fetch(url, {
          method: 'post',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(body),
        })

        const data = await res.json()
        if (data.error) {
          alert(data.error)
          return
        }
        // Almacenar token:
        const payload = {
          token: data.token
        }
        window.localStorage.setItem('token', JSON.stringify(payload))
        this.$router.push('/home')

      } catch (err) {

      }
    }
  }
}
</script>
