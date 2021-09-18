<template>
  <div class="author-create">
    <p>author-create</p>
    <input v-model="firstName" placeholder="First name" />
    <input v-model="lastName" placeholder="last name" />
    <textarea v-model="description" placeholder="description" />
    <button type="button" @click="onClick">Submit</button>
  </div>
</template>

<script>
export default {
  data () {
    return {
      firstName: '',
      lastName: '',
      description: '',
    }
  },
  methods: {
    async onClick () {
      const url = 'http://localhost:4500/api/author/create'

      const body = JSON.stringify({
        firstName: this.firstName,
        lastName: this.lastName,
        description: this.description,
      })

      try {
        const res = await fetch(url, {
          method: 'post',
          headers: {
            'Content-Type': 'application/json'
          },
          body
        })

        const data = await res.json()
        if (data.error) {
          alert(data.error)
          return
        }
        this.$router.push('/home')
      } catch (err) {
        alert('Hubo un problema al crear un author')
      }
    }
  }
}
</script>

