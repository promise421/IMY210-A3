<template>
  <div class="page">
    <header class="hero">
      <h1 class="logo">The Daily Read</h1>
      <p class="tagline">Stories worth your time</p>
      <nav class="nav">
        <NuxtLink to="/">Home</NuxtLink>
        <NuxtLink to="/search">Search</NuxtLink>
      </nav>
    </header>

    <main class="container">
      <div class="filter-bar">
        <label>Filter by category</label>
        <select v-model="selectedCategory">
          <option value="">All Categories</option>
          <option v-for="cat in categories" :key="cat" :value="cat">{{ cat }}</option>
        </select>
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
          <p class="snippet">{{ post.Snippet }}</p>
          <p class="author">By {{ post.Author }}</p>
        </NuxtLink>
      </div>

      <p v-if="filteredPosts.length === 0" class="no-results">No posts found.</p>
    </main>
  </div>
</template>

<script setup>
const { data } = await useFetch('http://localhost:1337/api/posts')

const posts = computed(() => data.value?.data || [])

const selectedCategory = ref('')

const categories = computed(() => {
  const cats = posts.value.map(p => p.category).filter(Boolean)
  return [...new Set(cats)]
})

const filteredPosts = computed(() => {
  if (!selectedCategory.value) return posts.value
  return posts.value.filter(p => p.category === selectedCategory.value)
})
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700;900&family=Source+Serif+4:wght@300;400&display=swap');

* { box-sizing: border-box; margin: 0; padding: 0; }

.page {
  min-height: 100vh;
  background: #f5f0e8;
  font-family: 'Source Serif 4', serif;
  color: #1a1a1a;
}

.hero {
  background: #1a1a1a;
  color: #f5f0e8;
  text-align: center;
  padding: 3rem 2rem 2rem;
  border-bottom: 4px solid #c8a96e;
}

.logo {
  font-family: 'Playfair Display', serif;
  font-size: 3.5rem;
  font-weight: 900;
  letter-spacing: -1px;
  color: #f5f0e8;
}

.tagline {
  color: #c8a96e;
  font-size: 1rem;
  letter-spacing: 3px;
  text-transform: uppercase;
  margin: 0.5rem 0 1.5rem;
}

.nav {
  display: flex;
  justify-content: center;
  gap: 2rem;
}

.nav a {
  color: #f5f0e8;
  text-decoration: none;
  font-size: 0.9rem;
  letter-spacing: 2px;
  text-transform: uppercase;
  border-bottom: 2px solid transparent;
  padding-bottom: 2px;
  transition: border-color 0.2s;
}

.nav a:hover { border-color: #c8a96e; }

.container {
  max-width: 1100px;
  margin: 0 auto;
  padding: 3rem 2rem;
}

.filter-bar {
  display: flex;
  align-items: center;
  gap: 1rem;
  margin-bottom: 2.5rem;
  padding-bottom: 1.5rem;
  border-bottom: 1px solid #d4c9b0;
}

.filter-bar label {
  font-size: 0.85rem;
  letter-spacing: 2px;
  text-transform: uppercase;
  color: #666;
}

.filter-bar select {
  padding: 0.5rem 1rem;
  border: 1px solid #d4c9b0;
  background: #fff;
  font-family: 'Source Serif 4', serif;
  font-size: 0.95rem;
  cursor: pointer;
  outline: none;
}

.posts-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 2rem;
}

.post-card {
  background: #fff;
  padding: 2rem;
  text-decoration: none;
  color: inherit;
  border: 1px solid #e0d9cc;
  border-top: 4px solid #c8a96e;
  transition: transform 0.2s, box-shadow 0.2s;
}

.post-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 24px rgba(0,0,0,0.1);
}

.category-tag {
  font-size: 0.75rem;
  letter-spacing: 2px;
  text-transform: uppercase;
  color: #c8a96e;
  font-weight: 600;
}

.post-card h2 {
  font-family: 'Playfair Display', serif;
  font-size: 1.4rem;
  margin: 0.75rem 0;
  line-height: 1.3;
}

.snippet {
  font-size: 0.95rem;
  color: #555;
  line-height: 1.6;
  margin-bottom: 1rem;
}

.author {
  font-size: 0.85rem;
  color: #999;
  font-style: italic;
}

.no-results {
  text-align: center;
  color: #999;
  padding: 3rem;
}
</style>