<template>
  <canvas ref="canvasEl" class="starfield" />
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

const canvasEl = ref<HTMLCanvasElement | null>(null)

interface Star {
  x: number
  y: number
  size: number
  opacity: number
  speed: number
  twinkleSpeed: number
  twinkleDir: number
}

let animFrameId = 0
let stars: Star[] = []
let resizeHandler: (() => void) | null = null

function randomBetween(min: number, max: number) {
  return Math.random() * (max - min) + min
}

function createStar(canvasWidth: number, canvasHeight: number, randomY = true): Star {
  const roll = Math.random()
  let size: number
  if (roll < 0.65) {
    size = randomBetween(0.5, 1)
  } else if (roll < 0.9) {
    size = randomBetween(1, 2)
  } else {
    size = randomBetween(2, 3)
  }

  return {
    x: randomBetween(0, canvasWidth),
    y: randomY ? randomBetween(0, canvasHeight) : canvasHeight + randomBetween(0, 20),
    size,
    opacity: randomBetween(0.2, 1.0),
    speed: randomBetween(0.1, 0.8),
    twinkleSpeed: randomBetween(0.003, 0.012),
    twinkleDir: Math.random() < 0.5 ? 1 : -1,
  }
}

function initStars(canvas: HTMLCanvasElement) {
  stars = Array.from({ length: 150 }, () => createStar(canvas.width, canvas.height))
}

function resize(canvas: HTMLCanvasElement) {
  canvas.width = window.innerWidth
  canvas.height = window.innerHeight
  initStars(canvas)
}

function draw() {
  const canvas = canvasEl.value
  if (!canvas) return
  const ctx = canvas.getContext('2d')
  if (!ctx) return

  ctx.clearRect(0, 0, canvas.width, canvas.height)

  for (const star of stars) {
    star.y -= star.speed

    if (star.y < -star.size) {
      star.x = randomBetween(0, canvas.width)
      star.y = canvas.height + star.size
    }

    star.opacity += star.twinkleSpeed * star.twinkleDir
    if (star.opacity >= 1.0) {
      star.opacity = 1.0
      star.twinkleDir = -1
    } else if (star.opacity <= 0.2) {
      star.opacity = 0.2
      star.twinkleDir = 1
    }

    ctx.beginPath()
    ctx.arc(star.x, star.y, star.size, 0, Math.PI * 2)
    ctx.fillStyle = `rgba(240, 240, 255, ${star.opacity})`
    ctx.fill()
  }

  animFrameId = requestAnimationFrame(draw)
}

onMounted(() => {
  const canvas = canvasEl.value
  if (!canvas) return
  resize(canvas)
  draw()
  resizeHandler = () => resize(canvas)
  window.addEventListener('resize', resizeHandler)
})

onUnmounted(() => {
  if (animFrameId) cancelAnimationFrame(animFrameId)
  if (resizeHandler) window.removeEventListener('resize', resizeHandler)
})
</script>

<style scoped>
.starfield {
  position: fixed;
  inset: 0;
  z-index: 1;
  pointer-events: none;
}
</style>
