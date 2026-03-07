<template>
  <div class="relative min-h-screen bg-[#0a0a0a] overflow-hidden">

    <!-- Grid background -->
    <div class="absolute inset-0 bg-grid"></div>
    <div class="absolute inset-0 bg-gradient-to-b from-transparent via-[#0a0a0a]/50 to-[#0a0a0a]"></div>

    <!-- Entrance wipe rectangles -->
    <div class="absolute inset-0 z-30 flex pointer-events-none" aria-hidden="true">
      <div
        v-for="i in RECT_COUNT"
        :key="i"
        class="wipe-rect"
        :ref="el => { if (el) rectEls[i - 1] = el as HTMLElement }"
      />
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
const RECT_COUNT = 25          // number of vertical strips
const WIPE_COLOR = '#111111'   // strip fill colour (slightly lighter than bg)
const ACCENT = '#e63946'      // accent colour for text and line  

// ─── Template refs ────────────────────────────────────────────────────────────
const rectEls    = ref<HTMLElement[]>([])
const titleEl    = ref<HTMLElement | null>(null)
const eyebrowEl  = ref<HTMLElement | null>(null)
const subtitleEl = ref<HTMLElement | null>(null)
const lineEl     = ref<HTMLElement | null>(null)

// ─── Animation ────────────────────────────────────────────────────────────────
onMounted(() => {
  const tl = createTimeline({ defaults: { ease: 'inOutSine' } })

  tl.add(rectEls.value, {
    scaleX:   [0, 1],
    duration: 700,
    delay:    stagger(55, { from: 'last' }),
    transformOrigin: ['50% 0%', '50% 0%'],
  })

  .add(rectEls.value, {
    scaleX:   [1, 0],
    duration: 650,
    delay:    stagger(45, { from: 'first' }),
    transformOrigin: ['50% 0%', '50% 0%'],
  }, '+=220')

  // ④ Eyebrow fades + rises
  .add(eyebrowEl.value!, {
    opacity:     [0, 1],
    translateY:  [18, 0],
    duration:    500,
    ease:        'easeOutExpo',
  }, '-=300')

  // ⑤ Title slams up
  .add(titleEl.value!, {
    opacity:     [0, 1],
    translateY:  [40, 0],
    duration:    700,
    ease:        'easeOutExpo',
  }, '-=380')

  // ⑥ Divider line grows
  .add(lineEl.value!, {
    scaleX:    [0, 1],
    opacity:   [0, 1],
    duration:  500,
    ease:      'easeOutExpo',
  }, '-=500')

  // ⑦ Subtitle fades
  .add(subtitleEl.value!, {
    opacity:     [0, 1],
    translateY:  [12, 0],
    duration:    500,
    ease:        'easeOutExpo',
  }, '-=350')
})
</script>

<style scoped>
/* ── Grid ───────────────────────────────────────────────────────────────────── */
.bg-grid {
  background-size: 70px 70px;
  background-image:
    linear-gradient(to right,  rgba(255,255,255,0.05) 1px, transparent 1px),
    linear-gradient(to bottom, rgba(255,255,255,0.05) 1px, transparent 1px);
  background-position: top center;
}

/* ── Wipe strips ────────────────────────────────────────────────────────────── */
.wipe-rect {
  flex: 1;
  height: 100%;
  background: v-bind(WIPE_COLOR);
  transform: scaleY(0);
  transform-origin: 50% 0%;
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
</style>