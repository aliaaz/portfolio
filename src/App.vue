<script setup>
import { ref, onMounted } from 'vue'

const name = ref('Alia Atheerah')
const email = ref('aliaatheerah.zulkifli@gmail.com')
const projects = ref([])
const loading = ref(true)
const error = ref(null)

onMounted(async () => {
  try {
    // We fetch from the root because 'public' content is served at '/'
    const response = await fetch('./projects.json')
    projects.value = await response.json()
  } catch (err) {
    error.value = true
    console.error("Failed to load projects:", err)
  } finally {
    loading.value = false
  }
})

const scrollTo = (id) => {
  document.getElementById(id).scrollIntoView({ behavior: 'smooth' })
}

const openNewPage = (url) => {
  window.open(url, '_blank');
}
</script>

<template>
  <div class="portfolio-wrapper">
    <nav class="nav-bar">
      <div class="container nav-content">
        <span class="logo">{{ name }}</span>
        <div class="nav-links">
          <button @click="scrollTo('projects')">Work</button>
          <button @click="scrollTo('contact')">Contact</button>
        </div>
      </div>
    </nav>

    <header class="hero container">
      <p class="badge">Software Developer</p>
      <h1>Building clean code <br><span class="highlight">for the modern web.</span></h1>
      <p class="subtitle">I'm a developer focused on performance and high-quality user experiences. Currently building
        with Vue 3 and TypeScript.</p>
      <div class="cta-group">
        <button @click="scrollTo('projects')" class="btn-primary">View my work</button>
        <button @click="scrollTo('contact')" class="btn-secondary">Let's talk</button>
      </div>
    </header>

    <section id="projects" class="container">
      <h2 class="section-title">Selected Projects</h2>
      <div v-if="loading" class="grid">
        <div v-for="p in [1, 2, 3]" class="card">
          <div class="card-number">
            <div class="ccs-placeholder ccs-w20"></div>
          </div>
          <h3>
            <div class="ccs-placeholder ccs-w60"></div>
          </h3>
          <p>
          <div class="ccs-placeholder ccs-w90"></div>
          </p>
          <div class="tags">
            <span class="tag"></span>
          </div>
        </div>
      </div>
      <div v-else-if="error" class="grid">
        <div class="card">
          <p>Couldn't load projects right now—check out my <a href="https://github.com/aliaaz" target="_blank"
              rel="noopener noreferrer">GitHub</a> in the meantime</p>
        </div>
      </div>
      <div v-else class="grid">
        <div v-for="p in projects" :key="p.id" class="card" @click="p.url ? openNewPage(p.url) : ''">
          <div class="card-number">{{ p.id < 10 ? 0 : '' }}{{ p.id }}</div>
              <h3>{{ p.title }}</h3>
              <p>{{ p.desc }}</p>
              <div class="tags">
                <span v-for="tag in p.tech" :key="tag" class="tag">{{ tag }}</span>
              </div>
          </div>
        </div>
    </section>

    <section id="contact" class="footer-cta">
      <div class="container">
        <h2>Get in touch</h2>
        <p>Interested in collaborating? Drop me an email.</p>
        <a :href="'mailto:' + email" class="email-link">{{ email }}</a>
      </div>
    </section>
  </div>
</template>

<style scoped>
/* Standard CSS - Works everywhere without external dependencies */
div {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
}

.portfolio-wrapper {
  background-color: #ffffff;
  min-height: 100vh;
}

.container {
  max-width: 1000px;
  margin: 0 auto;
  padding: 0 2rem;
}

/* Nav */
.nav-bar {
  position: fixed;
  top: 0;
  width: 100%;
  background: rgba(255, 255, 255, 0.9);
  backdrop-filter: blur(10px);
  border-bottom: 1px solid #edf2f7;
  z-index: 100;
}

.nav-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 70px;
}

.logo {
  font-weight: 800;
  font-size: 1.2rem;
  color: #4f46e5;
}

.nav-links button {
  background: none;
  border: none;
  margin-left: 2rem;
  color: #4a5568;
  font-weight: 500;
  cursor: pointer;
}

/* Hero */
.hero {
  padding: 160px 2rem 100px;
  text-align: center;
}

.badge {
  color: #4f46e5;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 1px;
  font-size: 0.8rem;
  margin-bottom: 1rem;
}

h1 {
  font-size: 3.5rem;
  font-weight: 900;
  line-height: 1.1;
  color: #1a202c;
  margin-bottom: 1.5rem;
}

.highlight {
  color: #4f46e5;
}

.subtitle {
  font-size: 1.2rem;
  color: #4a5568;
  max-width: 600px;
  margin: 0 auto 2.5rem;
  line-height: 1.6;
}

/* Buttons */
.cta-group {
  display: flex;
  justify-content: center;
  gap: 1rem;
}

.btn-primary {
  background: #4f46e5;
  color: white;
  border: none;
  padding: 1rem 2rem;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
}

.btn-secondary {
  background: #f7fafc;
  color: #1a202c;
  border: 1px solid #e2e8f0;
  padding: 1rem 2rem;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
}

/* Grid */
.section-title {
  font-size: 2rem;
  margin-bottom: 3rem;
  border-left: 5px solid #4f46e5;
  padding-left: 1rem;
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 2rem;
  margin-bottom: 5rem;
}

.card {
  padding: 2.5rem;
  background: #f8fafc;
  border-radius: 16px;
  border: 1px solid #e2e8f0;
  transition: transform 0.2s;
}

.card:hover {
  transform: translateY(-5px);
  border-color: #4f46e5;
}

.card-number {
  color: #cbd5e0;
  font-weight: 800;
  margin-bottom: 1rem;
}

h3 {
  margin-bottom: 1rem;
  font-size: 1.5rem;
}

.tags {
  display: flex;
  gap: 0.5rem;
  margin-top: 1.5rem;
  flex-wrap: wrap;
}

.tag {
  font-size: 0.7rem;
  background: #e0e7ff;
  color: #4338ca;
  padding: 0.3rem 0.6rem;
  border-radius: 4px;
  font-weight: 600;
}

/* Footer */
.footer-cta {
  background: #1a202c;
  color: white;
  padding: 100px 0;
  text-align: center;
}

.footer-cta h2 {
  font-size: 2.5rem;
  margin-bottom: 1rem;
}

.email-link {
  font-size: 1.5rem;
  color: #818cf8;
  text-decoration: none;
  font-weight: 700;
}
</style>