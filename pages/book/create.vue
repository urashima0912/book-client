<template>
  <div class="book-create">
    <p>book-create</p>
    <form action="http://localhost:4500/api/book/create" enctype="multipart/form-data" method="post">
      <input placeholder="Title" name="title" />
      <input placeholder="ISBN" name="isbn" />
      <select name="authorId">
        <option v-for="author in authors" v-bind:value="author._id" :key="author._id">
          {{ author.firstName + ' ' + author.lastName }}
        </option>
      </select>
      <input type="file" name="photo" />
      <button type="submit">Submit</button>
    </form>
  </div>
</template>

<script>
export default {
  data () {
    return {
      title: '',
      isbn: '',
      selected: undefined,
    }
  },

  async asyncData() {
    const url = 'http://localhost:4500/api/author/get'
    const res = await fetch(url)
    const data = await res.json()
    return {
      authors: data.authors,
    }
  },
  methods: {
    async onClick () {
      try {
        if (!this.title || !this.isbn || !this.selected) {
          return 
        }

        const body = JSON.stringify({
          title: this.title,
          isbn: this.isbn,
          authorId: this.selected,
        })

        const res = await fetch(url, {
          method: 'post',
          headers: {
            'Content-Type': 'application/json'
          },
          body,
        })

      } catch (_) {
        alert('Problema al crear book')
      }
    }
  }
}

</script>
