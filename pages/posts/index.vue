<template>
  <div v-if="posts">
    <v-card v-for="post in posts" :key="post.key" class="mx-auto" max-width="400">
      <v-img class="white--text align-end" height="200px" src="https://cdn.vuetifyjs.com/images/cards/docks.jpg">
        <v-card-title>{{ post.fields.title }}</v-card-title>
      </v-img>
      <v-card-subtitle class="pb-0">{{ post.fields.releaseDate }}</v-card-subtitle>
      <v-card-text class="text--primary">
        <div>{{ post.fields.myBlog.content[0].value }}</div>
      </v-card-text>
      <v-card-actions>
        <v-btn color="orange" text>
          <nuxt-link :to="{ name: 'posts-id', params: { id: post.sys.id } }">このブログを見てみる</nuxt-link>
        </v-btn>
      </v-card-actions>
    </v-card>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import { createClient } from '~/plugins/contentful.js'

const client = createClient()

export default Vue.extend({
  asyncData({ env }: any) {
    return Promise.all([
      client.getEntries({
        content_type: env.CTF_BLOG_POST_TYPE_ID,
        order: '-sys.createdAt'
      })
    ])
      .then(([posts]) => {
        return {
          posts: posts.items
        }
      })
      .catch(console.error)
  }
})
</script>
