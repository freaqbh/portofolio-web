<template>
  <section id="about" class="about-section relative z-10 py-28 px-8">
    <div class="max-w-6xl mx-auto">

      <!-- Section header -->
      <div class="about-header mb-16">
        <span class="section-index">02</span>
        <h2 class="section-title">About Me</h2>
        <div class="section-line" aria-hidden="true"></div>
      </div>

      <!-- Content grid -->
      <div class="grid grid-cols-1 md:grid-cols-2 gap-16 items-start">

        <!-- Left: Bio -->
        <div class="about-bio">
          <p class="bio-text mb-6">
            I'm a fullstack developer with 3+ years of experience building modern web applications. 
            I enjoy turning complex problems into simple, beautiful, and intuitive solutions.
          </p>
          <p class="bio-text mb-8">
            My focus is on creating performant and accessible applications using modern technologies. 
            I'm passionate about clean code, thoughtful architecture, and delivering experiences that users love.
          </p>

          <!-- Quick info grid -->
          <div class="info-grid">
            <div class="info-item">
              <span class="info-label">Location</span>
              <span class="info-value">Indonesia</span>
            </div>
            <div class="info-item">
              <span class="info-label">Experience</span>
              <span class="info-value">3+ Years</span>
            </div>
            <div class="info-item">
              <span class="info-label">Status</span>
              <span class="info-value info-value--green">Available</span>
            </div>
            <div class="info-item">
              <span class="info-label">Education</span>
              <span class="info-value">Computer Science</span>
            </div>
          </div>
        </div>

        <!-- Right: Tech stack -->
        <div class="tech-stack">
          <h3 class="stack-heading">Tech Stack</h3>
          <div class="stack-grid">
            <div v-for="tech in TECH_ITEMS" :key="tech.name" class="stack-card">
              <component :is="tech.icon" :size="20" class="stack-icon" />
              <span class="stack-name">{{ tech.name }}</span>
            </div>
          </div>
        </div>

      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { onMounted } from 'vue'
import { animate, stagger, onScroll } from 'animejs'
import {
  Code2,
  Database,
  Globe,
  Layers,
  Monitor,
  Server,
  Terminal,
  Palette,
  GitBranch,
  Container,
  Figma,
  Smartphone,
} from 'lucide-vue-next'

const TECH_ITEMS = [
  { name: 'Vue / Nuxt', icon: Code2 },
  { name: 'React / Next', icon: Layers },
  { name: 'TypeScript', icon: Terminal },
  { name: 'Node.js', icon: Server },
  { name: 'PostgreSQL', icon: Database },
  { name: 'MongoDB', icon: Database },
  { name: 'Tailwind CSS', icon: Palette },
  { name: 'REST / GraphQL', icon: Globe },
  { name: 'Docker', icon: Container },
  { name: 'Git', icon: GitBranch },
  { name: 'Figma', icon: Figma },
  { name: 'Responsive', icon: Smartphone },
]

onMounted(() => {
  // Section header entrance
  animate('.about-header', {
    opacity: [0, 1],
    translateY: [30, 0],
    duration: 800,
    ease: 'easeOutExpo',
    autoplay: onScroll({
      target: '.about-header',
      enter: 'bottom',
      leave: 'top',
    }),
  })

  // Section line draw
  animate('.section-line', {
    scaleX: [0, 1],
    duration: 1000,
    ease: 'easeOutExpo',
    autoplay: onScroll({
      target: '.section-line',
      enter: 'bottom',
      leave: 'top',
    }),
  })

  // Bio text fade in
  animate('.bio-text', {
    opacity: [0, 1],
    translateY: [20, 0],
    duration: 700,
    delay: stagger(150),
    ease: 'easeOutExpo',
    autoplay: onScroll({
      target: '.about-bio',
      enter: 'bottom',
      leave: 'top',
    }),
  })

  // Info items stagger
  animate('.info-item', {
    opacity: [0, 1],
    translateX: [-20, 0],
    duration: 600,
    delay: stagger(100),
    ease: 'easeOutExpo',
    autoplay: onScroll({
      target: '.info-grid',
      enter: 'bottom',
      leave: 'top',
    }),
  })

  // Tech stack cards stagger
  animate('.stack-card', {
    opacity: [0, 1],
    translateY: [20, 0],
    scale: [0.9, 1],
    duration: 500,
    delay: stagger(60),
    ease: 'easeOutExpo',
    autoplay: onScroll({
      target: '.tech-stack',
      enter: 'bottom',
      leave: 'top',
    }),
  })
})
</script>

<style scoped>
/* ── Section header ────────────────────────────────────────────────────────── */
.about-section {
  min-height: 100vh;
  display: flex;
  align-items: center;
}

.about-header {
  display: flex;
  align-items: center;
  gap: 1rem;
  opacity: 0;
}

.section-index {
  font-family: 'DM Mono', monospace;
  font-size: 0.7rem;
  letter-spacing: 0.15em;
  color: #e63946;
  opacity: 0.6;
}

.section-title {
  font-family: 'Oxanium', sans-serif;
  font-size: 1.8rem;
  font-weight: 500;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  color: rgba(240, 240, 255, 0.9);
}

.section-line {
  flex: 1;
  height: 1px;
  background: linear-gradient(90deg, rgba(230, 57, 70, 0.4), transparent);
  transform: scaleX(0);
  transform-origin: 0% 50%;
}

/* ── Bio ───────────────────────────────────────────────────────────────────── */
.bio-text {
  font-family: 'DM Mono', monospace;
  font-size: 0.85rem;
  font-weight: 300;
  line-height: 1.85;
  letter-spacing: 0.02em;
  color: rgba(200, 200, 220, 0.55);
  opacity: 0;
}

/* ── Info grid ─────────────────────────────────────────────────────────────── */
.info-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 0;
}

.info-item {
  display: flex;
  flex-direction: column;
  gap: 4px;
  padding: 1rem 0;
  border-top: 1px solid rgba(230, 57, 70, 0.1);
  opacity: 0;
}

.info-label {
  font-family: 'DM Mono', monospace;
  font-size: 0.6rem;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  color: rgba(200, 200, 220, 0.35);
}

.info-value {
  font-family: 'Oxanium', sans-serif;
  font-size: 0.9rem;
  font-weight: 400;
  color: rgba(240, 240, 255, 0.8);
}

.info-value--green {
  color: #10d48e;
}

/* ── Tech stack ────────────────────────────────────────────────────────────── */
.stack-heading {
  font-family: 'DM Mono', monospace;
  font-size: 0.65rem;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: rgba(230, 57, 70, 0.6);
  margin-bottom: 1.25rem;
}

.stack-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 8px;
}

@media (max-width: 768px) {
  .stack-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

.stack-card {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 0.75rem 1rem;
  background: rgba(255, 255, 255, 0.02);
  border: 1px solid rgba(255, 255, 255, 0.04);
  transition: border-color 0.25s ease, background 0.25s ease, box-shadow 0.25s ease;
  opacity: 0;
}

.stack-card:hover {
  border-color: rgba(230, 57, 70, 0.25);
  background: rgba(230, 57, 70, 0.04);
  box-shadow: 0 0 20px rgba(230, 57, 70, 0.06);
}

.stack-icon {
  color: #e63946;
  opacity: 0.7;
  flex-shrink: 0;
}

.stack-name {
  font-family: 'DM Mono', monospace;
  font-size: 0.72rem;
  letter-spacing: 0.04em;
  color: rgba(240, 240, 255, 0.65);
}
</style>
