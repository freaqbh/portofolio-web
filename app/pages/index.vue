<template>

<!-- butuh sync timeline, dan design hero section -->


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

    <div class="absolute inset-0 flex items-center justify-center z-40 pointer-events-none" aria-hidden="true">
      <svg viewBox="0 0 350 150" class="w-auto h-20 translate-x-4">
        <g stroke="#e63946" fill="none" fill-rule="evenodd" stroke-linejoin="round" stroke-width="2">
          <path transform="translate(1, 8)" class="line" d="M0.5 67V0.5H18V66C19 70 21.5 71.5 23.5 72H62V89H24.5C7.7 87.8 1.5 73.8333 0.5 67Z"/>
          <path transform="translate(29, 28)" class="line" d="M35 51.5C48.6 50.3 51.6667 39.6667 51.5 34.5C51.1 21.7 40.3333 17.8333 35 17.5C21.8 17.5 17 28.5 17.5 34.5C18.7 48.9 29.6667 51.8333 35 51.5ZM35 51.5V68.5M35 68.5C24.1667 68.5 2.1 61.7 0.5 34.5C0.5 20.5 7.8 1.7 35 0.5C45.3333 0.5 66.4 7.3 68 34.5C68.8333 44.5 63.4 65.3 35 68.5Z"/>
          <path class="line" transform="translate(80, 28)" d="M43.0004 67.5191V49.5191C23.4004 55.9191 17.8338 41.8524 17.5004 34.0191C18.7004 20.4191 30.0004 17.3525 35.5004 17.5191C45.5004 17.9191 50.3338 26.3525 51.5004 30.5191V69.0191H68.0004V30.5191C66.0004 7.71912 46.0004 0.0625212 35.5004 0.519122C7.90043 1.71912 0.667094 23.3524 0.500427 34.0191C4.10043 66.0191 30.3338 69.6858 43.0004 67.5191Z"/>
          <path class="line" transform="translate(131, 28)" d="M43.0004 1.99991V19.9999C23.4004 13.5999 17.8338 27.6666 17.5004 35.4999C18.7004 49.0999 30.0004 52.1666 35.5004 51.9999C45.5004 51.5999 50.3338 43.1666 51.5004 38.9999V0.499908L68.0004 0.499908L68.0004 38.9999C66.0004 61.7999 46.0004 69.4565 35.5004 68.9999C7.90043 67.7999 0.667094 46.1666 0.500427 35.4999C4.10043 3.49992 30.3338 -0.166756 43.0004 1.99991Z"/>
          <path class="line" transform="translate(182, 28)" d="M17.5 0.5H0.5V68.5H17.5V0.5Z"/>
          <path class="line" transform="translate(182, 0)" d="M0.5 9.5C0.9 16.3 5.5 19 9.5 19C16 19 19 15 19 9.5C19 3 14.5 0.5 9.5 0.5C2.3 0.5 0.5 6.5 0.5 9.5Z" />
          <path class="line" transform="translate(199,28)" d="M0.5 0.541504V68.5H17.5V18C28.7 18.4 33.1667 27.5 34 32L34.5 68.5H51.5V32C48.3 7.2 28.1667 0.666667 18.5 0.5L0.5 0.541504Z"/>
          <path class="line" transform="translate(233, 28)" d="M43.0004 67.5191V49.5191C23.4004 55.9191 17.8338 41.8524 17.5004 34.0191C18.7004 20.4191 30.0004 17.3525 35.5004 17.5191C45.5004 17.9191 50.3338 26.3525 51.5004 30.5191V69.0191C48.7004 87.4344 33.3338 86.3461 26.0004 83.5V101.5C58.8004 105.9 67.6671 81.6794 68.0004 69.0191V30.5191C66.0004 7.71912 46.0004 0.062518 35.5004 0.519119C7.90043 1.71912 0.667094 23.3524 0.500427 34.0191C4.10043 66.0191 30.3338 69.6858 43.0004 67.5191Z" />
        </g>
        <!-- shadow -->
        <g stroke="#e63946" fill="none" fill-rule="evenodd" stroke-linejoin="round" stroke-width="2">
          <path transform="translate(1, 8)" class="line-shadow" d="M0.5 67V0.5H18V66C19 70 21.5 71.5 23.5 72H62V89H24.5C7.7 87.8 1.5 73.8333 0.5 67Z"/>
          <path transform="translate(29, 28)" class="line-shadow" d="M35 51.5C48.6 50.3 51.6667 39.6667 51.5 34.5C51.1 21.7 40.3333 17.8333 35 17.5C21.8 17.5 17 28.5 17.5 34.5C18.7 48.9 29.6667 51.8333 35 51.5ZM35 51.5V68.5M35 68.5C24.1667 68.5 2.1 61.7 0.5 34.5C0.5 20.5 7.8 1.7 35 0.5C45.3333 0.5 66.4 7.3 68 34.5C68.8333 44.5 63.4 65.3 35 68.5Z"/>
          <path class="line-shadow" transform="translate(80, 28)" d="M43.0004 67.5191V49.5191C23.4004 55.9191 17.8338 41.8524 17.5004 34.0191C18.7004 20.4191 30.0004 17.3525 35.5004 17.5191C45.5004 17.9191 50.3338 26.3525 51.5004 30.5191V69.0191H68.0004V30.5191C66.0004 7.71912 46.0004 0.0625212 35.5004 0.519122C7.90043 1.71912 0.667094 23.3524 0.500427 34.0191C4.10043 66.0191 30.3338 69.6858 43.0004 67.5191Z"/>
          <path class="line-shadow" transform="translate(131, 28)" d="M43.0004 1.99991V19.9999C23.4004 13.5999 17.8338 27.6666 17.5004 35.4999C18.7004 49.0999 30.0004 52.1666 35.5004 51.9999C45.5004 51.5999 50.3338 43.1666 51.5004 38.9999V0.499908L68.0004 0.499908L68.0004 38.9999C66.0004 61.7999 46.0004 69.4565 35.5004 68.9999C7.90043 67.7999 0.667094 46.1666 0.500427 35.4999C4.10043 3.49992 30.3338 -0.166756 43.0004 1.99991Z"/>
          <path class="line-shadow" transform="translate(182, 28)" d="M17.5 0.5H0.5V68.5H17.5V0.5Z"/>
          <path class="line-shadow" transform="translate(182, 0)" d="M0.5 9.5C0.9 16.3 5.5 19 9.5 19C16 19 19 15 19 9.5C19 3 14.5 0.5 9.5 0.5C2.3 0.5 0.5 6.5 0.5 9.5Z" />
          <path class="line-shadow" transform="translate(199,28)" d="M0.5 0.541504V68.5H17.5V18C28.7 18.4 33.1667 27.5 34 32L34.5 68.5H51.5V32C48.3 7.2 28.1667 0.666667 18.5 0.5L0.5 0.541504Z"/>
          <path class="line-shadow" transform="translate(233, 28)" d="M43.0004 67.5191V49.5191C23.4004 55.9191 17.8338 41.8524 17.5004 34.0191C18.7004 20.4191 30.0004 17.3525 35.5004 17.5191C45.5004 17.9191 50.3338 26.3525 51.5004 30.5191V69.0191C48.7004 87.4344 33.3338 86.3461 26.0004 83.5V101.5C58.8004 105.9 67.6671 81.6794 68.0004 69.0191V30.5191C66.0004 7.71912 46.0004 0.062518 35.5004 0.519119C7.90043 1.71912 0.667094 23.3524 0.500427 34.0191C4.10043 66.0191 30.3338 69.6858 43.0004 67.5191Z" />
        </g>
      </svg>
    </div>

    <!-- Loading indicator -->
    <div ref="loaderEl" class="loader-overlay">
      <div class="loader-dots">
        <span class="loader-dot" />
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
import { animate, createTimeline, stagger, svg } from 'animejs'

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
    y: '3rem',
    duration: 300,
    delay: stagger(200, { from: 'center'}),
    ease: 'inOutSine',
  }, '+=500')

  tl.add( '.line-shadow', {
    opacity: [0, 0.2],
    duration: 300,
    ease: 'easeOutExpo',
  }, '-=300')

  tl.add( svg.createDrawable('.line'), {
    draw: ['0 0', '0 1', '1 1'],
    ease: 'inOutQuad',
    duration: 1000,
    delay: stagger(100),
    loop: 2
  }, '-=100')

  // ② Loading dots pulse animation
  tl.add('.loader-dot', {
    scale:    [1, 1.3, 1],
    opacity:  [1, 0.6, 1],
    duration: 1000,
    delay:    stagger(100, { from: 'first' }),
    ease:     'inOutSine',
    loop: 2
  }, '+=200')

  // ③ Fade out loader
  tl.add(loaderEl.value!, {
    opacity:  [1, 0],
    duration: 300,
    ease:     'easeInExpo',
  }, '+=300')

  tl.add( '.line-shadow', {
    opacity: [0.2, 0],
    duration: 300,
    ease: 'easeOutExpo',
  }, '-=300')

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
.line-shadow {
  opacity: 0;
}

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