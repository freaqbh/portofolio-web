<template>
  <section id="skills" class="skills-section relative z-10 py-28 px-8">
    <div class="max-w-6xl mx-auto">

      <!-- Section header -->
      <div class="skills-header mb-16">
        <span class="section-index">04</span>
        <h2 class="section-title">Skills</h2>
        <div class="section-line" aria-hidden="true"></div>
      </div>

      <!-- Skills categories -->
      <div class="skills-categories">
        <div v-for="(category, ci) in SKILLS" :key="category.name" class="skill-category">
          <div class="category-header">
            <component :is="category.icon" :size="16" class="category-icon" />
            <h3 class="category-name">{{ category.name }}</h3>
          </div>

          <div class="skill-items">
            <div
              v-for="skill in category.items"
              :key="skill.name"
              class="skill-item"
            >
              <div class="skill-top">
                <span class="skill-name">{{ skill.name }}</span>
                <span class="skill-level">{{ skill.level }}%</span>
              </div>
              <div class="skill-bar-bg">
                <div
                  class="skill-bar-fill"
                  :style="{ '--target-width': skill.level + '%' }"
                ></div>
              </div>
            </div>
          </div>

          <!-- Divider (except last) -->
          <div v-if="ci < SKILLS.length - 1" class="category-divider" aria-hidden="true"></div>
        </div>
      </div>

    </div>
  </section>
</template>

<script setup lang="ts">
import { onMounted } from 'vue'
import { animate, stagger, onScroll } from 'animejs'
import { Monitor, Server, Wrench } from 'lucide-vue-next'

const SKILLS = [
  {
    name: 'Frontend',
    icon: Monitor,
    items: [
      { name: 'Vue.js / Nuxt', level: 90 },
      { name: 'React / Next.js', level: 85 },
      { name: 'TypeScript', level: 88 },
      { name: 'Tailwind CSS', level: 92 },
      { name: 'HTML / CSS', level: 95 },
    ],
  },
  {
    name: 'Backend',
    icon: Server,
    items: [
      { name: 'Node.js / Express', level: 85 },
      { name: 'PostgreSQL', level: 80 },
      { name: 'MongoDB', level: 78 },
      { name: 'REST APIs', level: 90 },
      { name: 'GraphQL', level: 72 },
    ],
  },
  {
    name: 'Tools & Others',
    icon: Wrench,
    items: [
      { name: 'Git / GitHub', level: 90 },
      { name: 'Docker', level: 70 },
      { name: 'Figma', level: 75 },
      { name: 'CI/CD', level: 68 },
      { name: 'Linux / CLI', level: 82 },
    ],
  },
]

onMounted(() => {
  // Header
  animate('.skills-header', {
    opacity: [0, 1],
    translateY: [30, 0],
    duration: 800,
    ease: 'easeOutExpo',
    autoplay: onScroll({
      target: '.skills-header',
      enter: 'bottom',
      leave: 'top',
    }),
  })

  // Header line
  animate('.skills-section .section-line', {
    scaleX: [0, 1],
    duration: 1000,
    ease: 'easeOutExpo',
    autoplay: onScroll({
      target: '.skills-section .section-line',
      enter: 'bottom',
      leave: 'top',
    }),
  })

  // Category headers stagger
  animate('.category-header', {
    opacity: [0, 1],
    translateX: [-20, 0],
    duration: 600,
    delay: stagger(150),
    ease: 'easeOutExpo',
    autoplay: onScroll({
      target: '.skills-categories',
      enter: 'bottom',
      leave: 'top',
    }),
  })

  // Skill items entrance
  animate('.skill-item', {
    opacity: [0, 1],
    translateY: [15, 0],
    duration: 500,
    delay: stagger(40),
    ease: 'easeOutExpo',
    autoplay: onScroll({
      target: '.skills-categories',
      enter: 'bottom',
      leave: 'top',
    }),
  })

  // Skill bar fill animation
  animate('.skill-bar-fill', {
    width: ['0%', 'var(--target-width)'],
    duration: 1000,
    delay: stagger(60),
    ease: 'easeOutExpo',
    autoplay: onScroll({
      target: '.skills-categories',
      enter: 'bottom',
      leave: 'top',
    }),
  })

  // Category dividers
  animate('.category-divider', {
    scaleX: [0, 1],
    duration: 800,
    delay: stagger(200),
    ease: 'easeOutExpo',
    autoplay: onScroll({
      target: '.skills-categories',
      enter: 'bottom',
      leave: 'top',
    }),
  })
})
</script>

<style scoped>
/* ── Section header ────────────────────────────────────────────────────────── */
.skills-header {
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

/* ── Categories layout ─────────────────────────────────────────────────────── */
.skills-categories {
  display: grid;
  grid-template-columns: 1fr;
  gap: 0;
}

.skill-category {
  padding: 2rem 0;
}

.skill-category:first-child {
  padding-top: 0;
}

.category-header {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 1.5rem;
  opacity: 0;
}

.category-icon {
  color: #e63946;
  opacity: 0.7;
}

.category-name {
  font-family: 'Oxanium', sans-serif;
  font-size: 1rem;
  font-weight: 500;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: rgba(240, 240, 255, 0.75);
}

/* ── Skill items ───────────────────────────────────────────────────────────── */
.skill-items {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 16px;
}

.skill-item {
  opacity: 0;
}

.skill-top {
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  margin-bottom: 6px;
}

.skill-name {
  font-family: 'DM Mono', monospace;
  font-size: 0.72rem;
  color: rgba(240, 240, 255, 0.6);
  letter-spacing: 0.04em;
}

.skill-level {
  font-family: 'DM Mono', monospace;
  font-size: 0.6rem;
  color: rgba(230, 57, 70, 0.5);
  letter-spacing: 0.08em;
}

/* ── Skill bar ─────────────────────────────────────────────────────────────── */
.skill-bar-bg {
  height: 3px;
  background: rgba(255, 255, 255, 0.04);
  overflow: hidden;
}

.skill-bar-fill {
  height: 100%;
  width: 0%;
  background: linear-gradient(90deg, #e63946, rgba(230, 57, 70, 0.4));
  box-shadow: 0 0 8px rgba(230, 57, 70, 0.3);
}

/* ── Divider ───────────────────────────────────────────────────────────────── */
.category-divider {
  height: 1px;
  background: linear-gradient(90deg, rgba(230, 57, 70, 0.15), transparent 70%);
  transform: scaleX(0);
  transform-origin: 0% 50%;
  margin-top: 0.5rem;
}
</style>
