<template>
  <nav ref="navEl" class="navbar">

    <!-- Logo -->
    <NuxtLink to="/" class="nav-logo" aria-label="Home">
      <svg viewBox="0 0 350 150" class="logo-svg" aria-hidden="true">
        <g stroke="#e63946" fill="none" fill-rule="evenodd" stroke-linejoin="round" stroke-width="2">
          <path transform="translate(1, 8)" d="M0.5 67V0.5H18V66C19 70 21.5 71.5 23.5 72H62V89H24.5C7.7 87.8 1.5 73.8333 0.5 67Z"/>
          <path transform="translate(29, 28)" d="M35 51.5C48.6 50.3 51.6667 39.6667 51.5 34.5C51.1 21.7 40.3333 17.8333 35 17.5C21.8 17.5 17 28.5 17.5 34.5C18.7 48.9 29.6667 51.8333 35 51.5ZM35 51.5V68.5M35 68.5C24.1667 68.5 2.1 61.7 0.5 34.5C0.5 20.5 7.8 1.7 35 0.5C45.3333 0.5 66.4 7.3 68 34.5C68.8333 44.5 63.4 65.3 35 68.5Z"/>
          <path transform="translate(80, 28)" d="M43.0004 67.5191V49.5191C23.4004 55.9191 17.8338 41.8524 17.5004 34.0191C18.7004 20.4191 30.0004 17.3525 35.5004 17.5191C45.5004 17.9191 50.3338 26.3525 51.5004 30.5191V69.0191H68.0004V30.5191C66.0004 7.71912 46.0004 0.0625212 35.5004 0.519122C7.90043 1.71912 0.667094 23.3524 0.500427 34.0191C4.10043 66.0191 30.3338 69.6858 43.0004 67.5191Z"/>
        </g>
      </svg>
    </NuxtLink>

    <!-- Desktop nav links -->
    <ul class="nav-links" role="list">
      <li v-for="item in NAV_ITEMS" :key="item.label" class="nav-item">
        <NuxtLink
          :to="item.href"
          class="nav-link"
          :class="{ 'is-active': activeSection === item.id }"
          @mouseenter="onLinkHover"
          @mouseleave="onLinkLeave"
        >
          <span class="link-label">{{ item.label }}</span>
          <span class="link-bar" aria-hidden="true" />
        </NuxtLink>
      </li>
    </ul>

    <!-- Right cluster -->
    <div class="nav-right">
      <span class="status-dot" aria-hidden="true" />
      <span class="status-text">Available</span>
      <a href="#contact" class="cta-btn" @mouseenter="onCtaHover" @mouseleave="onCtaLeave">
        <span class="cta-label">Get in touch</span>
        <svg class="cta-arrow" viewBox="0 0 16 16" fill="none" aria-hidden="true">
          <path d="M3 8h10M9 4l4 4-4 4" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
      </a>
    </div>

    <!-- Mobile hamburger -->
    <button
      class="hamburger"
      :class="{ 'is-open': mobileOpen }"
      :aria-expanded="mobileOpen"
      aria-label="Toggle menu"
      @click="toggleMobile"
    >
      <span class="ham-line" />
      <span class="ham-line" />
      <span class="ham-line" />
    </button>

    <!-- Mobile drawer -->
    <Transition name="drawer">
      <div v-if="mobileOpen" class="mobile-drawer" role="dialog" aria-modal="true">
        <ul class="mobile-links" role="list">
          <li
            v-for="(item, i) in NAV_ITEMS"
            :key="item.label"
            class="mobile-item"
            :style="{ '--i': i }"
          >
            <NuxtLink :to="item.href" class="mobile-link" @click="mobileOpen = false">
              <span class="mobile-index">{{ item.index }}</span>
              {{ item.label }}
            </NuxtLink>
          </li>
        </ul>
        <a href="#contact" class="mobile-cta" @click="mobileOpen = false">Get in touch →</a>
      </div>
    </Transition>

    <!-- Scan line overlay -->
    <div class="scan-line" aria-hidden="true" />
  </nav>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'
import { animate, stagger } from 'animejs'

// ── Config ──────────────────────────────────────────────────────────────────
const NAV_ITEMS = [
  { id: 'home',    label: 'Home',    href: '/',        index: '01' },
  { id: 'about',   label: 'About',   href: '#about',   index: '02' },
  { id: 'work',    label: 'Work',    href: '#work',    index: '03' },
  { id: 'skills',  label: 'Skills',  href: '#skills',  index: '04' },
  { id: 'contact', label: 'Contact', href: '#contact', index: '05' },
]

// ── State ───────────────────────────────────────────────────────────────────
const navEl        = ref<HTMLElement | null>(null)
const mobileOpen   = ref(false)
const activeSection = ref('home')
const scrolled     = ref(false)

let started = false
// ── Scroll handling ─────────────────────────────────────────────────────────
function onScroll() {
  scrolled.value = window.scrollY > 24
}

let scrollListener: (() => void) | null = null

// ── Hover micro-animations ───────────────────────────────────────────────────
function onLinkHover(e: MouseEvent) {
  const bar = (e.currentTarget as HTMLElement).querySelector('.link-bar')
  if (!bar) return
  animate(bar, { scaleX: [0, 1], duration: 250, ease: 'easeOutExpo' })
}

function onLinkLeave(e: MouseEvent) {
  const bar = (e.currentTarget as HTMLElement).querySelector('.link-bar')
  if (!bar) return
  animate(bar, { scaleX: [1, 0], duration: 200, ease: 'easeInExpo' })
}

function onCtaHover(e: MouseEvent) {
  const arrow = (e.currentTarget as HTMLElement).querySelector('.cta-arrow')
  if (!arrow) return
  animate(arrow, { translateX: [0, 4], duration: 200, ease: 'easeOutExpo' })
}

function onCtaLeave(e: MouseEvent) {
  const arrow = (e.currentTarget as HTMLElement).querySelector('.cta-arrow')
  if (!arrow) return
  animate(arrow, { translateX: [4, 0], duration: 200, ease: 'easeOutExpo' })
}

function toggleMobile() {
  mobileOpen.value = !mobileOpen.value
}

// ── Mount ────────────────────────────────────────────────────────────────────
onMounted(() => {
  scrollListener = onScroll
  window.addEventListener('scroll', scrollListener, { passive: true })

    if (started) {
        console.log('Playing navbar animation')

        // entrance animation
        animate(navEl.value!, {
            opacity:   [0, 1],
            translateY: [-20, 0],
            duration:  600,
            ease:      'easeOutExpo',
            delay:     800,
        })

        animate('.nav-item', {
            opacity:   [0, 1],
            translateY: [-8, 0],
            duration:  500,
            delay:     stagger(60, { start: 1000 }),
            ease:      'easeOutExpo',
        })
    }
})

onUnmounted(() => {
  if (scrollListener) window.removeEventListener('scroll', scrollListener)
})

function play() {
    started = true

    // entrance animation
    animate(navEl.value!, {
        opacity:   [0, 1],
        translateY: [-20, 0],
        duration:  600,
        ease:      'easeOutExpo',
        delay:     800,
    })

    animate('.nav-item', {
        opacity:   [0, 1],
        translateY: [-8, 0],
        duration:  500,
        delay:     stagger(60, { start: 1000 }),
        ease:      'easeOutExpo',
    })
}

defineExpose({ play })

</script>

<style scoped>
/* ── Base ─────────────────────────────────────────────────────────────────── */
.navbar {
  position: fixed;
  top: 0; left: 0; right: 0;
  z-index: 100;
  display: flex;
  align-items: center;
  gap: 0;
  padding: 0 2rem;
  height: 84px;
  opacity: 0;

  backdrop-filter: blur(8px) saturate(1.4);
  -webkit-backdrop-filter: blur(8px) saturate(1.4);
  border-bottom: 1px solid rgba(255, 255, 255, 0.06);
  box-shadow: 0 1px 0 rgba(230, 57, 70, 0.08);
}

/* ── Logo ─────────────────────────────────────────────────────────────────── */
.nav-logo {
  display: flex;
  align-items: center;
  flex-shrink: 0;
  margin-right: auto;
  text-decoration: none;
}
.logo-svg {
  width: auto;
  height: 40px;
  filter: drop-shadow(0 0 6px rgba(230, 57, 70, 0.4));
  transition: filter 0.3s ease;
}
.nav-logo:hover .logo-svg {
  filter: drop-shadow(0 0 12px rgba(230, 57, 70, 0.7));
}

/* ── Desktop links ────────────────────────────────────────────────────────── */
.nav-links {
  display: flex;
  align-items: center;
  gap: 0.25rem;
  list-style: none;
  margin: 0;
  padding: 0;
}

.nav-item {
  opacity: 0;
}

.nav-link {
  position: relative;
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 0.45rem 0.85rem;
  text-decoration: none;
  color: rgba(240, 240, 255, 0.5);
  font-family: 'Courier New', 'Menlo', monospace;
  font-size: 0.80rem;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  transition: color 0.2s ease;
  outline: none;
}

.nav-link:hover,
.nav-link:focus-visible,
.nav-link.is-active {
  color: rgba(240, 240, 255, 0.95);
}

.nav-link.is-active .link-index {
  color: #e63946;
}

.link-index {
  font-size: 0.6rem;
  color: rgba(230, 57, 70, 0.5);
  font-family: 'Courier New', monospace;
  letter-spacing: 0.05em;
  transition: color 0.2s;
  user-select: none;
}

.nav-link:hover .link-index {
  color: #e63946;
}

.link-label {
  position: relative;
  z-index: 1;
}

.link-bar {
  position: absolute;
  bottom: 4px; left: 0.85rem; right: 0.85rem;
  height: 1px;
  background: #e63946;
  transform: scaleX(0);
  transform-origin: 0% 50%;
  box-shadow: 0 0 6px rgba(230, 57, 70, 0.6);
}

/* ── Right cluster ────────────────────────────────────────────────────────── */
.nav-right {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  margin-left: 1.5rem;
}

.status-dot {
  width: 6px; height: 6px;
  border-radius: 50%;
  background: #10d48e;
  box-shadow: 0 0 8px rgba(16, 212, 142, 0.8);
  animation: pulse-dot 2.4s ease-in-out infinite;
}

@keyframes pulse-dot {
  0%, 100% { box-shadow: 0 0 6px rgba(16,212,142,0.8); }
  50%       { box-shadow: 0 0 14px rgba(16,212,142,0.5); }
}

.status-text {
  font-family: 'Courier New', monospace;
  font-size: 0.62rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: rgba(16, 212, 142, 0.7);
}

/* ── CTA button ───────────────────────────────────────────────────────────── */
.cta-btn {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 0.42rem 1rem;
  text-decoration: none;
  font-family: 'Courier New', monospace;
  font-size: 0.80rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: #e63946;
  border: 1px solid rgba(230, 57, 70, 0.4);
  transition: background 0.2s, border-color 0.2s, color 0.2s,
              box-shadow 0.2s;
  position: relative;
  overflow: hidden;
}

.cta-btn::before {
  content: '';
  position: absolute;
  inset: 0;
  background: rgba(230, 57, 70, 0.08);
  transform: scaleX(0);
  transform-origin: 0% 50%;
  transition: transform 0.25s ease;
}

.cta-btn:hover::before { transform: scaleX(1); }
.cta-btn:hover {
  border-color: rgba(230, 57, 70, 0.8);
  box-shadow: 0 0 16px rgba(230, 57, 70, 0.5),
              inset 0 0 16px rgba(230, 57, 70, 0.07);
}

.cta-arrow {
  width: 16px; height: 16px;
  flex-shrink: 0;
}

/* ── Hamburger ────────────────────────────────────────────────────────────── */
.hamburger {
  display: none;
  flex-direction: column;
  gap: 5px;
  padding: 8px;
  background: none;
  border: none;
  cursor: pointer;
  margin-left: 1rem;
}

.ham-line {
  display: block;
  width: 22px;
  height: 1.5px;
  background: rgba(240, 240, 255, 0.7);
  transition: transform 0.3s ease, opacity 0.3s ease;
  transform-origin: center;
}

.hamburger.is-open .ham-line:nth-child(1) { transform: translateY(6.5px) rotate(45deg); }
.hamburger.is-open .ham-line:nth-child(2) { opacity: 0; transform: scaleX(0); }
.hamburger.is-open .ham-line:nth-child(3) { transform: translateY(-6.5px) rotate(-45deg); }

/* ── Mobile drawer ────────────────────────────────────────────────────────── */
.mobile-drawer {
  position: fixed;
  top: 64px; left: 0; right: 0;
  background: rgba(10, 10, 10, 0.97);
  backdrop-filter: blur(20px);
  border-bottom: 1px solid rgba(230, 57, 70, 0.15);
  padding: 2rem;
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.mobile-links {
  list-style: none;
  margin: 0; padding: 0;
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
}

.mobile-item {
  animation: slideIn 0.35s calc(var(--i) * 60ms) both ease;
}

@keyframes slideIn {
  from { opacity: 0; transform: translateX(-12px); }
  to   { opacity: 1; transform: translateX(0); }
}

.mobile-link {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.75rem 0;
  text-decoration: none;
  color: rgba(240, 240, 255, 0.6);
  font-family: 'Courier New', monospace;
  font-size: 0.85rem;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  border-bottom: 1px solid rgba(255,255,255,0.04);
  transition: color 0.2s;
}

.mobile-link:hover { color: rgba(240, 240, 255, 0.95); }

.mobile-index {
  font-size: 0.6rem;
  color: rgba(230, 57, 70, 0.5);
}

.mobile-cta {
  align-self: flex-start;
  padding: 0.6rem 1.4rem;
  text-decoration: none;
  font-family: 'Courier New', monospace;
  font-size: 0.75rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: #e63946;
  border: 1px solid rgba(230, 57, 70, 0.4);
  transition: background 0.2s;
  animation: slideIn 0.35s calc(5 * 60ms) both ease;
}

.mobile-cta:hover { background: rgba(230, 57, 70, 0.1); }

/* ── Drawer transitions ───────────────────────────────────────────────────── */
.drawer-enter-active,
.drawer-leave-active { transition: opacity 0.25s ease, transform 0.25s ease; }
.drawer-enter-from,
.drawer-leave-to { opacity: 0; transform: translateY(-8px); }

/* ── Scan line ─────────────────────────────────────────────────────────────── */
.scan-line {
  position: absolute;
  bottom: 0; left: 0; right: 0;
  height: 1px;
  background: linear-gradient(90deg,
    transparent 0%,
    rgba(230, 57, 70, 0.0) 20%,
    rgba(230, 57, 70, 0.5) 50%,
    rgba(230, 57, 70, 0.0) 80%,
    transparent 100%);
  animation: scan 4s ease-in-out infinite;
}

@keyframes scan {
  0%, 100% { opacity: 0.3; transform: scaleX(0.4); }
  50%       { opacity: 1;   transform: scaleX(1); }
}

/* ── Responsive ───────────────────────────────────────────────────────────── */
@media (max-width: 768px) {
  .nav-links,
  .nav-right { display: none; }
  .hamburger { display: flex; }
  .navbar    { padding: 0 1.25rem; }
}
</style>