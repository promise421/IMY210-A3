<template>
  <div class="page">
    <header class="hero">
      <h1 class="logo">The Daily Read</h1>
      <nav class="nav">
        <NuxtLink to="/">Home</NuxtLink>
        <NuxtLink to="/search">Search</NuxtLink>
      </nav>
    </header>

    <main class="container">
      <h2 class="page-title">Search Posts</h2>

      <div class="search-bar">
        <input
          v-model="query"
          type="text"
          placeholder="Search by title or author..."
        />
      </div>

      <div class="posts-grid">
        <NuxtLink
          v-for="post in filteredPosts"
          :key="post.id"
          :to="`/post/${post.documentId}`"
          class="post-card"
        >
          <span class="category-tag">{{ post.category }}</span>
          <h2>{{ post.Title }}</h2>
          <p class="author">By {{ post.Author }}</p>
        </NuxtLink>
      </div>

      <p v-if="query && filteredPosts.length === 0" class="no-results">
        No posts found for "{{ query }}"
      </p>

      <p v-if="!query" class="hint">Start typing to search...</p>
    </main>
  </div>
</template>

<script setup>
const { data } = await useFetch('http://localhost:1337/api/posts')
const posts = computed(() => data.value?.data || [])
const query = ref('')

const filteredPosts = computed(() => {
  if (!query.value) return []
  const q = query.value.toLowerCase()
  return posts.value.filter(p =>
    p.Title?.toLowerCase().includes(q) ||
    p.Author?.toLowerCase().includes(q)
  )
})
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

.container { max-width: 900px; margin: 0 auto; padding: 3rem 2rem; }

.page-title { font-family: 'Playfair Display', serif; font-size: 2rem; margin-bottom: 2rem; }

.search-bar input {
  width: 100%;
  padding: 1rem 1.5rem;
  font-size: 1.1rem;
  font-family: 'Source Serif 4', serif;
  border: 2px solid #d4c9b0;
  background: #fff;
  outline: none;
  margin-bottom: 2.5rem;
  transition: border-color 0.2s;
}

.search-bar input:focus { border-color: #c8a96e; }

.posts-grid { display: grid; gap: 1.5rem; }

.post-card { background: #fff; padding: 1.5rem 2rem; text-decoration: none; color: inherit; border: 1px solid #e0d9cc; border-left: 4px solid #c8a96e; transition: transform 0.2s; display: block; }

.post-card:hover { transform: translateX(6px); }

.category-tag { font-size: 0.75rem; letter-spacing: 2px; text-transform: uppercase; color: #c8a96e; font-weight: 600; }

.post-card h2 { font-family: 'Playfair Display', serif; font-size: 1.3rem; margin: 0.5rem 0; }

.author { font-size: 0.85rem; color: #999; font-style: italic; }

.no-results, .hint { text-align: center; color: #999; padding: 3rem; }
</style>