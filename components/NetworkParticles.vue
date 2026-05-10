<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

interface Particle {
  x: number
  y: number
  vx: number
  vy: number
  radius: number
  opacity: number
}

const canvasRef = ref<HTMLCanvasElement | null>(null)
let animationId: number
let particles: Particle[] = []
const PARTICLE_COUNT = 55
const CONNECTION_DISTANCE = 140
const SPEED = 0.3

function initParticles(w: number, h: number) {
  particles = []
  for (let i = 0; i < PARTICLE_COUNT; i++) {
    particles.push({
      x: Math.random() * w,
      y: Math.random() * h,
      vx: (Math.random() - 0.5) * SPEED,
      vy: (Math.random() - 0.5) * SPEED,
      radius: Math.random() * 2 + 1.5,
      opacity: Math.random() * 0.4 + 0.3,
    })
  }
}

function draw(ctx: CanvasRenderingContext2D, w: number, h: number) {
  ctx.clearRect(0, 0, w, h)

  // Update positions
  for (const p of particles) {
    p.x += p.vx
    p.y += p.vy
    if (p.x < 0 || p.x > w) p.vx *= -1
    if (p.y < 0 || p.y > h) p.vy *= -1
  }

  // Draw connections
  for (let i = 0; i < particles.length; i++) {
    for (let j = i + 1; j < particles.length; j++) {
      const dx = particles[i].x - particles[j].x
      const dy = particles[i].y - particles[j].y
      const dist = Math.sqrt(dx * dx + dy * dy)
      if (dist < CONNECTION_DISTANCE) {
        const alpha = (1 - dist / CONNECTION_DISTANCE) * 0.25
        ctx.strokeStyle = `rgba(27, 58, 92, ${alpha})`
        ctx.lineWidth = 0.8
        ctx.beginPath()
        ctx.moveTo(particles[i].x, particles[i].y)
        ctx.lineTo(particles[j].x, particles[j].y)
        ctx.stroke()
      }
    }
  }

  // Draw particles
  for (const p of particles) {
    // Glow
    const gradient = ctx.createRadialGradient(p.x, p.y, 0, p.x, p.y, p.radius * 4)
    gradient.addColorStop(0, `rgba(232, 145, 58, ${p.opacity * 0.3})`)
    gradient.addColorStop(1, 'rgba(232, 145, 58, 0)')
    ctx.fillStyle = gradient
    ctx.beginPath()
    ctx.arc(p.x, p.y, p.radius * 4, 0, Math.PI * 2)
    ctx.fill()

    // Core
    ctx.fillStyle = `rgba(27, 58, 92, ${p.opacity})`
    ctx.beginPath()
    ctx.arc(p.x, p.y, p.radius, 0, Math.PI * 2)
    ctx.fill()
  }

  animationId = requestAnimationFrame(() => draw(ctx, w, h))
}

onMounted(() => {
  const canvas = canvasRef.value
  if (!canvas) return
  const ctx = canvas.getContext('2d')
  if (!ctx) return

  const resize = () => {
    const dpr = window.devicePixelRatio || 1
    const rect = canvas.parentElement?.getBoundingClientRect() || canvas.getBoundingClientRect()
    canvas.width = rect.width * dpr
    canvas.height = rect.height * dpr
    canvas.style.width = `${rect.width}px`
    canvas.style.height = `${rect.height}px`
    ctx.setTransform(dpr, 0, 0, dpr, 0, 0)
    initParticles(rect.width, rect.height)
  }

  resize()
  draw(ctx, canvas.width / (window.devicePixelRatio || 1), canvas.height / (window.devicePixelRatio || 1))
  window.addEventListener('resize', resize)
})

onUnmounted(() => {
  cancelAnimationFrame(animationId)
})
</script>

<template>
  <canvas
    ref="canvasRef"
    class="network-particles"
  />
</template>

<style scoped>
.network-particles {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  z-index: 0;
  pointer-events: none;
}
</style>
