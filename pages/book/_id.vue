<template>
  <div class="book-id">
    <p>book-id</p>
    <nuxt-link to="/home">home</nuxt-link>
    <p>{{ data }}</p>
    <img :src="data.book.photo" style="width: 300px;" />
    <input v-model="data.book.title" />
    <select v-model="authorId">
      <option v-for="author in authors" v-bind:value="author._id" :key="author._id">
        {{ author.firstName + ' ' + author.lastName }}
      </option>
    </select>
    <button type="button" @click="onClick">Update</button>
  </div>
</template>

<script>
export default {
  async asyncData (ctx) {
    try {
      const id = ctx.params.id
      const url = `http://localhost:4500/api/book/${id}`
      const res = await fetch(url)
      const data = await res.json()

      const url2 = 'http://localhost:4500/api/author/get'
      const res2 = await fetch(url2)
      const data2 = await res2.json()

      return {
        data,
        authors: data2.authors,
        authorId: data.book.author._id,
        bookId: ctx.params.id,
      }

    } catch (_) {
      alert('Hubo un problema!')
      ctx.redirect('/home')
    }
  },
  methods: {
    async onClick () {
      const url = `http://localhost:4500/api/book/${this.bookId}`

      const body = JSON.stringify({
        title: this.data.book.title,
        authorId: this.authorId,
      })

      try {
        const res = await fetch(url, {
          method: 'put',
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
        alert('Hubo un problema al actualizar un book')
      }
    }
  }
}
</script>
