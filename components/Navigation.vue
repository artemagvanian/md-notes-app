<template>
  <p v-if="$fetchState.pending">Fetching navigation...</p>
  <p v-else-if="$fetchState.error">An error occurred :(</p>
  <div v-else class="px-10">
    <h3 class="text-3xl font-bold my-4">Navigate</h3>
    <ul class="list-disc list-inside">
      <li v-if="path">
        <NuxtLink to="/" class="text-blue-500">Home</NuxtLink>
      </li>
      <li v-for="item in items" :key="item.title">
        <NuxtLink :to="item.path" class="text-blue-500">{{
          item.title
        }}</NuxtLink>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: { path: { type: String, default: '/' } },
  data() {
    return {
      items: [],
    }
  },
  async fetch() {
    const content = await this.$content(`/${this.path}`)
      .only(['title', 'dir'])
      .fetch()
    if (Array.isArray(content)) {
      this.items = content
    } else {
      this.items = await this.$content(content.dir)
        .only(['title', 'dir'])
        .fetch()
    }
  },
}
</script>
