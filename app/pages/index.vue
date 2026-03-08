<template>

  <div class="relative min-h-screen bg-[#0a0a0a] overflow-hidden">
    <div class="orb orb-1"></div>
    <div class="orb orb-2"></div>
    <!-- Grid background -->
    <div class="absolute inset-0 grid-lines" aria-hidden="true"></div>
    <div 
      v-for="i in GRID_COLS"
      :key="'v' + i"
      class="grid-line-v"
      :style="{ left: `${(i / (GRID_COLS)) * 100}%` }"
    ></div>

    <div 
      v-for="i in GRID_ROWS"
      :key="'h' + i"
      class="grid-line-h"
      :style="{ top: `${(i / (GRID_ROWS)) * 100}%` }"
    ></div>


    <div class="absolute inset-0 bg-gradient-to-b from-transparent via-[#0a0a0a]/50 to-[#0a0a0a]"></div>

    <!-- Entrance wipe rectangles -->
    <div class="absolute inset-0 flex flex-col z-30 pointer-events-none" aria-hidden="true">
      <div
        v-for="i in RECT_COUNT"
        :key="i"
        class="wipe-rect"
        :ref="el => { if (el) rectEls[i - 1] = el as HTMLElement }"
      />
    </div>

    <!-- Loading indicator -->
    <div ref="loaderEl" class="loader-overlay">
      <div class="loader-dots">
        <span class="loader-dot" />
        <span class="loader-dot" />
        <span class="loader-dot" />
        <span class="loader-dot" />
      </div>
    </div>

    <!-- Main content -->
    <div class="relative z-10 flex flex-col items-center justify-center min-h-screen gap-2">
      <p ref="eyebrowEl" class="eyebrow">CREATIVE DEVELOPER</p>
      <h1 ref="titleEl" class="title">PORTFOLIO</h1>
      <div ref="lineEl" class="divider" />
      <p ref="subtitleEl" class="subtitle">Design · Motion · Code</p>
    </div>

  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { animate, createTimeline, stagger } from 'animejs'

// ─── Config ───────────────────────────────────────────────────────────────────
const RECT_COUNT = 15       // number of vertical strips
const WIPE_COLOR = '#111111'   // strip fill colour (slightly lighter than bg)
const ACCENT = '#e63946'      // accent colour for text and line
const GRID_COLS = 14
const GRID_ROWS = 10


// ─── Template refs ────────────────────────────────────────────────────────────
const rectEls    = ref<HTMLElement[]>([])
const loaderEl   = ref<HTMLElement | null>(null) 
const titleEl    = ref<HTMLElement | null>(null)
const eyebrowEl  = ref<HTMLElement | null>(null)
const subtitleEl = ref<HTMLElement | null>(null)
const lineEl     = ref<HTMLElement | null>(null)

// ─── Animation ────────────────────────────────────────────────────────────────
onMounted(() => {
  const tl = createTimeline({ defaults: { ease: 'inOutExpo' } })

  // ① Loading dots pulse (plays while wipe rects are fully visible)
  tl.add('.loader-dot', {
    scale:    [0, 1],
    opacity:  [0, 1],
    duration: 400,
    delay:    stagger(120, { from: 'center'}),
    ease:     'easeOutExpo',
  })

  tl.add( '.loader-dot',{
    y: '2rem',
    duration: 300,
    delay: stagger(200, { from: 'center'}),
    ease: 'easeInExpo',
  }, '+=500')

  // ② Loading dots pulse animation
  tl.add('.loader-dot', {
    scale:    [1, 1.3, 1],
    opacity:  [1, 0.6, 1],
    duration: 1000,
    delay:    stagger(100, { from: 'center' }),
    ease:     'inOutSine',
  }, '+=200')

  // ③ Fade out loader
  tl.add(loaderEl.value!, {
    opacity:  [1, 0],
    duration: 300,
    ease:     'easeInExpo',
  }, '+=300')

  // ④ Wipe rects fade out from middle to sides
  tl.add(rectEls.value, {
    scaleX:   [1, 0],
    opacity: [1, 0],
    duration: 800,
    delay:    stagger(45, { from: 'center' }),
    transformOrigin: ['50% 0%', '50% 0%'],
  }, '-=100')

  // ⑤ Eyebrow fades + rises
  tl.add(eyebrowEl.value!, {
    opacity:     [0, 1],
    translateY:  [18, 0],
    duration:    500,
    ease:        'easeOutExpo',
  }, '-=300')

  // ⑥ Title slams up
  tl.add(titleEl.value!, {
    opacity:     [0, 1],
    translateY:  [40, 0],
    duration:    700,
    ease:        'easeOutExpo',
  }, '-=380')

  // ⑦ Divider line grows
  tl.add(lineEl.value!, {
    scaleX:    [0, 1],
    opacity:   [0, 1],
    duration:  500,
    ease:      'easeOutExpo',
  }, '-=500')

  // ⑧ Subtitle fades
  tl.add(subtitleEl.value!, {
    opacity:     [0, 1],
    translateY:  [12, 0],
    duration:    500,
    ease:        'easeOutExpo',
  }, '-=350')

  // ⑨ Orbs fade in
  tl.add('.orb', {
    opacity: [0, 1],
    duration: 1200,
    delay: stagger(200, { from: 'last' }),
  }, '-=800')

  // grid animation
  tl.add('.grid-line-v', {
    scaleY:   [0, 1],
    duration: 900,
    delay:    stagger(40, { from: 'last' }),
    transformOrigin: ['50% 0%', '50% 0%'],
    ease: 'easeOutExpo',
  }, '-=800')

  tl.add('.grid-line-h', {
    scaleX:   [0, 1],
    duration: 900,
    delay:    stagger(40, { from: 'last' }),
    transformOrigin: ['0% 50%', '0% 50%'],
    ease: 'easeOutExpo',
  }, '-=1000')
})
</script>

<style scoped>
/* ── Grid ───────────────────────────────────────────────────────────────────── */
.grid-lines {
  pointer-events: none;
  z-index: 0;
}

.grid-line-v {
  position: absolute;
  top: 0;
  width: 1px;
  height: 100%;
  background: rgba(255, 255, 255, 0.05);
  transform: scaleY(0);
  transform-origin: 50% 0%;
}

.grid-line-h {
  position: absolute;
  top: 0;
  width: 100%;
  height: 1px;
  background: rgba(255, 255, 255, 0.05);
  transform: scaleX(0);
  transform-origin: 0% 50%;
}

/* ── Wipe strips ────────────────────────────────────────────────────────────── */
.wipe-rect {
  flex: 1;
  background: v-bind(WIPE_COLOR);
  transform: scaleX(1);
  transform-origin: 50% 0%;
  opacity: 1;
}

/* ── Loader ─────────────────────────────────────────────────────────────────── */
.loader-overlay {
  position: fixed;
  inset: 0;
  z-index: 40;
  display: flex;
  align-items: center;
  justify-content: center;
  pointer-events: none;
}

.loader-dots {
  display: flex;
  gap: 12px;
}

.loader-dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: v-bind(ACCENT);
  opacity: 0;
  transform: scale(0);
}

/* ── Content ────────────────────────────────────────────────────────────────── */
.eyebrow {
  font-family: 'Courier New', monospace;
  font-size: 0.7rem;
  letter-spacing: 0.35em;
  color: v-bind(ACCENT);
  text-transform: uppercase;
  opacity: 0;
}

.title {
  font-family: 'Georgia', serif;
  font-size: clamp(3rem, 10vw, 7rem);
  font-weight: 900;
  letter-spacing: 0.18em;
  color: #ffffff;
  line-height: 1;
  opacity: 0;
}

.divider {
  width: 80px;
  height: 1px;
  background: v-bind(ACCENT);
  transform-origin: left center;
  opacity: 0;
}

.subtitle {
  font-family: 'Courier New', monospace;
  font-size: 0.75rem;
  letter-spacing: 0.25em;
  color: rgba(255, 255, 255, 0.35);
  text-transform: uppercase;
  opacity: 0;
}

/* ── FLOATING ORBS ── */
.orb {
  position: fixed; border-radius: 50%;
  filter: blur(70px);
  pointer-events: none; 
  opacity: 0;
  }
.orb-1 { width: 500px; height: 500px; top: -150px; right: -100px; background: rgba(255,75,110,0.12); z-index: 0; }
.orb-2 { width: 400px; height: 400px; bottom: -100px; left: -80px; background: rgba(10,255,210,0.08); z-index: 1;}
</style>