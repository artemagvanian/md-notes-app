<template>
  <main class="grid grid-cols-4 gap-4 w-screen h-5/6 bg-gray-50">
    <aside
      class="border-2 border-l-0 rounded rounded-l-none p-4 my-3 bg-gray-200"
    >
      <Navigation :path="$route.params.pathMatch" />
    </aside>
    <article class="col-span-3 overflow-y-auto">
      <h1 class="text-6xl font-bold my-10 text-center">{{ article.title }}</h1>
      <nuxt-content
        :document="article"
        class="prose prose-sm sm:prose lg:prose-lg xl:prose-xl mx-auto"
      />
    </article>
  </main>
</template>

<script>
export default {
  async asyncData({ $content, params, error }) {
    const path = `/${params.pathMatch || 'index'}`
    const [article] = await $content({ deep: true }).where({ path }).fetch()

    if (!article) {
      return error({ statusCode: 404, message: 'Article not found' })
    }

    return {
      article,
    }
  },
}
</script>
