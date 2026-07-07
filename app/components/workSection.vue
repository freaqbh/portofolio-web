<template>
  <section id="work" class="work-section relative z-10 py-28 px-8">
    <div class="max-w-6xl mx-auto">

      <!-- Section header -->
      <div class="work-header mb-16">
        <span class="section-index">03</span>
        <h2 class="section-title">Selected Work</h2>
        <div class="section-line" aria-hidden="true"></div>
      </div>

      <!-- Projects grid -->
      <div class="projects-grid">
        <div
          v-for="(project, i) in PROJECTS"
          :key="project.title"
          class="project-card"
          :style="{ '--idx': i }"
          @mouseenter="onCardHover($event)"
          @mouseleave="onCardLeave($event)"
        >
          <!-- Preview area -->
          <div class="project-preview">
            <div class="preview-gradient" :style="{ background: project.gradient }"></div>
            <span class="preview-number">{{ String(i + 1).padStart(2, '0') }}</span>
          </div>

          <!-- Info -->
          <div class="project-info">
            <h3 class="project-title">{{ project.title }}</h3>
            <p class="project-desc">{{ project.desc }}</p>
            <div class="project-tags">
              <span v-for="tag in project.tags" :key="tag" class="project-tag">{{ tag }}</span>
            </div>
          </div>

          <!-- Hover arrow -->
          <div class="project-arrow" aria-hidden="true">
            <svg viewBox="0 0 16 16" fill="none">
              <path d="M3 8h10M9 4l4 4-4 4" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
          </div>
        </div>
      </div>

    </div>
  </section>
</template>

<script setup lang="ts">
import { onMounted } from 'vue'
import { animate, stagger, onScroll } from 'animejs'

const PROJECTS = [
  {
    title: 'E-Commerce Platform',
    desc: 'Full-stack e-commerce solution with real-time inventory, payment integration, and admin dashboard.',
    tags: ['Vue.js', 'Node.js', 'PostgreSQL', 'Stripe'],
    gradient: 'linear-gradient(135deg, rgba(230,57,70,0.15) 0%, rgba(40,15,18,0.4) 100%)',
  },
  {
    title: 'Task Management App',
    desc: 'Collaborative project management tool with drag-and-drop boards, real-time updates, and team chat.',
    tags: ['React', 'TypeScript', 'Socket.io', 'MongoDB'],
    gradient: 'linear-gradient(135deg, rgba(16,212,142,0.12) 0%, rgba(10,30,25,0.4) 100%)',
  },
  {
    title: 'Portfolio CMS',
    desc: 'Custom content management system for creative professionals with visual editor and theme builder.',
    tags: ['Nuxt.js', 'Tailwind', 'Prisma', 'Vercel'],
    gradient: 'linear-gradient(135deg, rgba(100,100,255,0.1) 0%, rgba(20,15,40,0.4) 100%)',
  },
  {
    title: 'Analytics Dashboard',
    desc: 'Real-time analytics platform with interactive charts, custom reports, and data export capabilities.',
    tags: ['Next.js', 'D3.js', 'Redis', 'AWS'],
    gradient: 'linear-gradient(135deg, rgba(255,180,50,0.1) 0%, rgba(40,30,10,0.4) 100%)',
  },
]

function onCardHover(e: MouseEvent) {
  const card = e.currentTarget as HTMLElement
  const arrow = card.querySelector('.project-arrow')
  if (arrow) {
    animate(arrow, { opacity: [0, 1], translateX: [-8, 0], duration: 250, ease: 'easeOutExpo' })
  }
}

function onCardLeave(e: MouseEvent) {
  const card = e.currentTarget as HTMLElement
  const arrow = card.querySelector('.project-arrow')
  if (arrow) {
    animate(arrow, { opacity: [1, 0], translateX: [0, 8], duration: 200, ease: 'easeInExpo' })
  }
}

onMounted(() => {
  // Header entrance
  animate('.work-header', {
    opacity: [0, 1],
    translateY: [30, 0],
    duration: 800,
    ease: 'easeOutExpo',
    autoplay: onScroll({
      target: '.work-header',
      enter: 'bottom',
      leave: 'top',
    }),
  })

  // Header line
  animate('.work-section .section-line', {
    scaleX: [0, 1],
    duration: 1000,
    ease: 'easeOutExpo',
    autoplay: onScroll({
      target: '.work-section .section-line',
      enter: 'bottom',
      leave: 'top',
    }),
  })

  // Project cards stagger
  animate('.project-card', {
    opacity: [0, 1],
    translateY: [40, 0],
    duration: 700,
    delay: stagger(120),
    ease: 'easeOutExpo',
    autoplay: onScroll({
      target: '.projects-grid',
      enter: 'bottom',
      leave: 'top',
    }),
  })
})
</script>

<style scoped>
/* ── Section header ────────────────────────────────────────────────────────── */
.work-header {
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

/* ── Projects grid ─────────────────────────────────────────────────────────── */
.projects-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 16px;
}

@media (max-width: 768px) {
  .projects-grid {
    grid-template-columns: 1fr;
  }
}

.project-card {
  position: relative;
  display: flex;
  flex-direction: column;
  background: rgba(255, 255, 255, 0.015);
  border: 1px solid rgba(255, 255, 255, 0.04);
  overflow: hidden;
  cursor: pointer;
  transition: border-color 0.3s ease, box-shadow 0.3s ease, transform 0.3s ease;
  opacity: 0;
}

.project-card:hover {
  border-color: rgba(230, 57, 70, 0.2);
  box-shadow: 0 8px 40px rgba(230, 57, 70, 0.06), 0 0 0 1px rgba(230, 57, 70, 0.08);
  transform: translateY(-2px);
}

/* ── Preview area ──────────────────────────────────────────────────────────── */
.project-preview {
  position: relative;
  height: 200px;
  overflow: hidden;
}

.preview-gradient {
  position: absolute;
  inset: 0;
  transition: transform 0.5s ease;
}

.project-card:hover .preview-gradient {
  transform: scale(1.05);
}

.preview-number {
  position: absolute;
  top: 16px;
  right: 16px;
  font-family: 'DM Mono', monospace;
  font-size: 0.6rem;
  letter-spacing: 0.15em;
  color: rgba(240, 240, 255, 0.2);
}

/* ── Info ───────────────────────────────────────────────────────────────────── */
.project-info {
  padding: 1.5rem;
  flex: 1;
  display: flex;
  flex-direction: column;
}

.project-title {
  font-family: 'Oxanium', sans-serif;
  font-size: 1.1rem;
  font-weight: 500;
  color: rgba(240, 240, 255, 0.9);
  margin-bottom: 0.5rem;
}

.project-desc {
  font-family: 'DM Mono', monospace;
  font-size: 0.72rem;
  line-height: 1.7;
  color: rgba(200, 200, 220, 0.45);
  margin-bottom: 1rem;
  flex: 1;
}

/* ── Tags ───────────────────────────────────────────────────────────────────── */
.project-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
}

.project-tag {
  font-family: 'DM Mono', monospace;
  font-size: 0.58rem;
  letter-spacing: 0.08em;
  padding: 3px 10px;
  background: rgba(230, 57, 70, 0.06);
  border: 1px solid rgba(230, 57, 70, 0.12);
  color: rgba(230, 57, 70, 0.7);
  text-transform: uppercase;
}

/* ── Hover arrow ───────────────────────────────────────────────────────────── */
.project-arrow {
  position: absolute;
  top: 16px;
  left: 16px;
  width: 20px;
  height: 20px;
  color: #e63946;
  opacity: 0;
}
</style>
