<template>
  <div class="page">
    <header class="hero">
      <h1 class="logo">The Daily Read</h1>
      <nav class="nav">
        <NuxtLink to="/">Home</NuxtLink>
        <NuxtLink to="/search">Search</NuxtLink>
      </nav>
    </header>

    <main class="container" v-if="post">
      <NuxtLink to="/" class="back">← Back to all posts</NuxtLink>
      <span class="category-tag">{{ post.category }}</span>
      <h1 class="title">{{ post.Title }}</h1>
      <p class="meta">By <strong>{{ post.Author }}</strong> · {{ formatDate(post.publishedAt) }}</p>
      <hr class="divider" />
      <div class="content">
        <div v-for="(block, i) in post.content" :key="i">
          <p v-if="block.type === 'paragraph'">
            <span v-for="(child, j) in block.children" :key="j"
              :style="{ fontWeight: child.bold ? 'bold' : 'normal' }">
              {{ child.text }}
            </span>
          </p>
          <h2 v-else-if="block.type === 'heading'">
            <span v-for="(child, j) in block.children" :key="j">{{ child.text }}</span>
          </h2>
        </div>
      </div>
    </main>

    <p v-else class="loading">Loading post...</p>
  </div>
</template>

<script setup>
const route = useRoute()
const { data } = await useFetch(`http://localhost:1337/api/posts/${route.params.id}`)
const post = computed(() => data.value?.data || null)

const formatDate = (dateStr) => {
  if (!dateStr) return ''
  return new Date(dateStr).toLocaleDateString('en-ZA', { year: 'numeric', month: 'long', day: 'numeric' })
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700;900&family=Source+Serif+4:wght@300;400&display=swap');

* { box-sizing: border-box; margin: 0; padding: 0; }

.page { min-height: 100vh; background: #f5f0e8; font-family: 'Source Serif 4', serif; color: #1a1a1a; }

.hero { background: #1a1a1a; color: #f5f0e8; text-align: center; padding: 2rem; border-bottom: 4px solid #c8a96e; }

.logo { font-family: 'Playfair Display', serif; font-size: 2.5rem; font-weight: 900; color: #f5f0e8; }

.nav { display: flex; justify-content: center; gap: 2rem; margin-top: 1rem; }

.nav a { color: #f5f0e8; text-decoration: none; font-size: 0.9rem; letter-spacing: 2px; text-transform: uppercase; border-bottom: 2px solid transparent; padding-bottom: 2px; transition: border-color 0.2s; }

.nav a:hover { border-color: #c8a96e; }

.container { max-width: 750px; margin: 0 auto; padding: 3rem 2rem; }

.back { display: inline-block; margin-bottom: 2rem; color: #c8a96e; text-decoration: none; font-size: 0.9rem; letter-spacing: 1px; }

.back:hover { text-decoration: underline; }

.category-tag { font-size: 0.75rem; letter-spacing: 2px; text-transform: uppercase; color: #c8a96e; font-weight: 600; }

.title { font-family: 'Playfair Display', serif; font-size: 2.8rem; line-height: 1.2; margin: 1rem 0; }

.meta { color: #888; font-size: 0.95rem; margin-bottom: 1.5rem; }

.divider { border: none; border-top: 2px solid #d4c9b0; margin-bottom: 2rem; }

.content { font-size: 1.1rem; line-height: 1.9; color: #333; }

.content p { margin-bottom: 1.5rem; }

.content h2 { font-family: 'Playfair Display', serif; font-size: 1.6rem; margin: 2rem 0 1rem; }

.loading { text-align: center; padding: 4rem; color: #999; }
</style>