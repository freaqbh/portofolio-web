<template>
  <nav class="navbar">
    <div class="navbar-inner">
      <a href="#home" class="navbar-logo" aria-label="Navigate to home">
        <span class="logo-bracket">[</span>
        <span class="logo-text">PORTFOLIO</span>
        <span class="logo-bracket">]</span>
      </a>
      <ul class="navbar-links">
        <li v-for="link in links" :key="link.href">
          <a :href="link.href" class="navbar-link" :aria-label="`Navigate to ${link.label.toLowerCase()} section`">
            <span class="link-number" aria-hidden="true">{{ link.number }}</span>
            {{ link.label }}
          </a>
        </li>
      </ul>
    </div>
  </nav>
</template>

<script setup lang="ts">
const links = [
  { href: '#home',    label: 'HOME',    number: '01' },
  { href: '#about',   label: 'ABOUT',   number: '02' },
  { href: '#service', label: 'SERVICE', number: '03' },
  { href: '#contact', label: 'CONTACT', number: '04' },
]
</script>

<style scoped>
.navbar {
  position: sticky;
  top: 0;
  z-index: 50;
  width: 100%;
  background: rgba(10, 10, 10, 0.85);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border-bottom: 1px solid rgba(255, 255, 255, 0.06);
}

.navbar-inner {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 2rem;
  height: 56px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.navbar-logo {
  font-family: 'Georgia', serif;
  font-size: 0.85rem;
  font-weight: 900;
  letter-spacing: 0.22em;
  color: #ffffff;
  text-decoration: none;
  display: flex;
  align-items: center;
  gap: 2px;
  transition: opacity 0.2s ease;
}

.navbar-logo:hover {
  opacity: 0.75;
}

.logo-bracket {
  color: #e63946;
  font-family: 'Courier New', monospace;
}

.logo-text {
  color: #ffffff;
}

.navbar-links {
  list-style: none;
  margin: 0;
  padding: 0;
  display: flex;
  gap: 2.5rem;
}

.navbar-link {
  font-family: 'Courier New', monospace;
  font-size: 0.65rem;
  letter-spacing: 0.3em;
  color: rgba(255, 255, 255, 0.5);
  text-decoration: none;
  text-transform: uppercase;
  display: flex;
  align-items: center;
  gap: 6px;
  transition: color 0.2s ease;
  position: relative;
}

.navbar-link::after {
  content: '';
  position: absolute;
  bottom: -4px;
  left: 0;
  width: 0;
  height: 1px;
  background: #e63946;
  transition: width 0.25s ease;
}

.navbar-link:hover {
  color: #ffffff;
}

.navbar-link:hover::after {
  width: 100%;
}

.link-number {
  color: #e63946;
  font-size: 0.55rem;
  opacity: 0.8;
}
</style>
