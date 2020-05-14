<template>
  <v-layout>
    <v-flex class="text-center">
      <template v-if="bodies.length != 0">
        <h1>{{ title }}</h1>
        <br />
        <div v-for="body in bodies" :key="body.value">
          {{ body.content[0].value }}
        </div>
        <br />
        <div>{{ releaseDate }}</div>
      </template>
      <template v-else>
        <v-overlay :value="overlay">
          <v-progress-circular indeterminate size="64"></v-progress-circular>
        </v-overlay>
      </template>
    </v-flex>
    <nuxt-link to="/posts">記事一覧に戻る</nuxt-link>>
  </v-layout>
</template>

<script lang="ts">
import Vue from 'vue'
import { createClient } from '~/plugins/contentful.js'

const client = createClient()

interface Data {
  postId: string
  title: string
  releaseDate: string
  bodies: []
}

export default Vue.extend({
  // validate({ params }) {
  //   return true
  // },
  data(): Data {
    return {
      postId: this.$nuxt.$route.params.id,
      title: '',
      releaseDate: '',
      bodies: []
    }
  },
  mounted(): Promise<void> {
    return Promise.all([
      client.getEntries({
        'sys.id': this.postId
      })
    ])
      .then(([entries]) => {
        this.title = entries.items[0].fields.title
        this.releaseDate = entries.items[0].fields.releaseDate
        this.bodies = entries.items[0].fields.myBlog.content
      })
      .catch(console.error)
  }
})
</script>
