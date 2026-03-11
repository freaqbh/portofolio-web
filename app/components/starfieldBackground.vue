<template>
  <canvas ref="canvasEl" class="starfield" />
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'
import { createTimeline, animate } from 'animejs'

const canvasEl = ref<HTMLCanvasElement | null>(null)

interface Star {
    x: number
    y: number
    size: number
    opacity: number
    speed: number
}

let animFrameId = 0
let stars: Star[] = []
let resizeHandler: (() => void) | null = null
let timeline: ReturnType<typeof createTimeline> | null = null
let started = false

function randomBetween(min: number, max: number) {
    return Math.random() * (max - min) + min
}

function createStar(canvasWidth: number, canvasHeight: number): Star {
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
        y: randomBetween(0, canvasHeight),
        size,
        opacity: 0,
        speed: randomBetween(0.1, 0.8),
    }
}

function buildTimeline() {
    if (timeline) timeline.cancel()

    timeline = createTimeline({
        autoplay: false, // parent controls playback
    })

    const lowOpacity = randomBetween(0.2, 0.4)
    const highOpacity = randomBetween(0.8, 1.0)
    const duration = randomBetween(800, 2500)

    for (const star of stars) {

        timeline.add(star, {
            opacity: [
                { to: lowOpacity, duration },
                { to: highOpacity, duration },
            ],
            ease: 'inOutSine',
            delay: randomBetween(0, 2000),
            }, 0)
    }

  return timeline
}

function initStars(canvas: HTMLCanvasElement) {
    stars = Array.from({ length: 150 }, () => createStar(canvas.width, canvas.height))
    buildTimeline()
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

    if (started) {
        for (const star of stars) {
            star.y -= star.speed

            if (star.y < -star.size) {
            star.x = randomBetween(0, canvas.width)
            star.y = canvas.height + star.size
            }

            ctx.beginPath()
            ctx.arc(star.x, star.y, star.size, 0, Math.PI * 2)
            ctx.fillStyle = `rgba(240, 240, 255, ${star.opacity})`
            ctx.fill()
        }
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
    if (timeline) timeline.cancel()
})

// Expose for parent to integrate into a master timeline
function getTimeline() {
    return timeline
}

function play() {
    started = true
    timeline?.play()
}

function pause() {
    timeline?.pause()
}

defineExpose({ getTimeline, play, pause })
</script>

<style scoped>
    .starfield {
        position: fixed;
        inset: 0;
        z-index: 0;
        pointer-events: none;
    }
</style>