<template>
  <div class="home">
    <p>home</p>
    <nuxt-link to="/author/create">Create author</nuxt-link>
    <nuxt-link to="/book/create">Create book</nuxt-link>

    <div v-for="author in authors" :key="author._id">
      <p><strong>{{ author.firstName + ' ' + author.lastName  }}</strong></p>
      <button type="button" @click="removeAuthor(author._id)">Remove</button>
      <button type="button" @click="updateAuthor(author._id)">update</button>
      <button type="button" @click="detailsAuthor(author._id)">details</button>
    </div>
    
    <div v-for="book in books" :key="book._id">
      <p><strong>{{ book.title }}</strong></p>
      <img :src="book.photo" style="width: 300px;" />
      <button type="button" @click="removeBook(book._id)">Remove</button>
      <button type="button" @click="updateBook(book._id)">update</button>
      <button type="button" @click="detailsBook(book._id)">details</button>
    </div>
  </div>
</template>

<script>
export default {
  async asyncData () {
    const url = 'http://localhost:4500/api/author/get'
    const res = await fetch(url)
    const data = await res.json()

    const url2 = 'http://localhost:4500/api/book/get'
    const res2 = await fetch(url2)
    const data2 = await res2.json()

    return {
      authors: data.authors,
      books: data2.books,
    }
  },
  methods: {
    async removeAuthor (id) {
      const url = `http://localhost:4500/api/author/${id}`
      await fetch(url, { method: 'delete'})
      
      await this.updateData()
    },
    async updateData () {
      const url2 = 'http://localhost:4500/api/author/get'
      const res = await fetch(url2)
      const data = await res.json()
      this.authors = data.authors

      const url3 = 'http://localhost:4500/api/book/get'
      const res2 = await fetch(url3)
      const data2 = await res2.json()
      this.books = data2.books
    },

    updateAuthor (id) {
      console.log('update: '+ id)
    },
    detailsAuthor (id) {
      console.log('update: '+ id)
    },
    async removeBook (id) {
      const url = `http://localhost:4500/api/book/${id}`
      await fetch(url, { method: 'delete'})

      await this.updateData()
    },
    updateBook (id) {
      console.log('update: '+ id)
    },
    detailsBook (id) {
      console.log('update: '+ id)
      this.$router.push(`/book/${id}`)
    },
  }
}
</script>