<template>
  <div class="sign-up">
    <p>sign-up</p>
    <input v-model="email" placeholder="email" />
    <input v-model="password1" type="password" placeholder="password" />
    <input v-model="password2" type="password" placeholder="password repeat" />
    <button type="button" v-on:click="onClick">Submit</button>
  </div>
</template>

<script>
export default {
  data () {
    return {
      email: '',
      password1: '',
      password2: '',
    }
  },
  methods: {
    async onClick () {
      if (!this.email || !this.password1 || !this.password2 || this.password1 !== this.password2) {
        alert('Error en email o password')
        return
      }

      // TODO.
      try {
        const url = 'http://localhost:4500/api/admin/sign-up'
        const body = {
          email: this.email,
          password1: this.password1,
          password2: this.password2,
        }

        const res = await fetch(url, {
          method: 'post',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(body)
        })

        const data = await res.json()
        if (data.error) {
          alert(data.error)
        } else {
          console.log({ data }) // data
          this.$router.push('/sign-in')
        }
      } catch (err) {
        alert('Hubo un error')
      }
    }
  }
}

</script>
