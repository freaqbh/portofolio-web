<template>
  <section id="contact" class="contact-section relative z-10 py-28 px-8">
    <div class="max-w-6xl mx-auto">

      <!-- Section header -->
      <div class="contact-header mb-16">
        <span class="section-index">05</span>
        <h2 class="section-title">Get In Touch</h2>
        <div class="section-line" aria-hidden="true"></div>
      </div>

      <!-- Content grid -->
      <div class="grid grid-cols-1 md:grid-cols-2 gap-16">

        <!-- Left: CTA + info -->
        <div class="contact-info">
          <h3 class="contact-headline">
            Let's build something<br />
            <span class="headline-accent">together</span>
          </h3>
          <p class="contact-desc">
            Have a project in mind or just want to chat? I'm always open to discussing new opportunities
            and creative ideas. Drop me a message and I'll get back to you as soon as possible.
          </p>

          <!-- Contact details -->
          <div class="contact-details">
            <a href="mailto:hello@example.com" class="contact-link">
              <Mail :size="16" />
              <span>hello@example.com</span>
            </a>
            <a href="https://github.com" target="_blank" rel="noopener noreferrer" class="contact-link">
              <Github :size="16" />
              <span>github.com/username</span>
            </a>
            <a href="https://linkedin.com" target="_blank" rel="noopener noreferrer" class="contact-link">
              <Linkedin :size="16" />
              <span>linkedin.com/in/username</span>
            </a>
          </div>
        </div>

        <!-- Right: Contact form -->
        <form class="contact-form" @submit.prevent>
          <div class="form-group">
            <label for="contact-name" class="form-label">Name</label>
            <input id="contact-name" type="text" class="form-input" placeholder="Your name" autocomplete="name" />
          </div>
          <div class="form-group">
            <label for="contact-email" class="form-label">Email</label>
            <input id="contact-email" type="email" class="form-input" placeholder="your@email.com" autocomplete="email" />
          </div>
          <div class="form-group">
            <label for="contact-message" class="form-label">Message</label>
            <textarea id="contact-message" class="form-input form-textarea" placeholder="Tell me about your project..." rows="5"></textarea>
          </div>
          <button type="submit" class="form-submit">
            <span>Send Message</span>
            <Send :size="14" />
          </button>
        </form>

      </div>

      <!-- Footer -->
      <footer class="site-footer">
        <div class="footer-line" aria-hidden="true"></div>
        <div class="footer-content">
          <span class="footer-copy">&copy; {{ currentYear }} M. Abhinaya Al Faruqi</span>
          <span class="footer-built">Built with Nuxt &amp; Anime.js</span>
        </div>
      </footer>

    </div>
  </section>
</template>

<script setup lang="ts">
import { computed, onMounted } from 'vue'
import { animate, stagger, onScroll } from 'animejs'
import { Mail, Github, Linkedin, Send } from 'lucide-vue-next'

const currentYear = computed(() => new Date().getFullYear())

onMounted(() => {
  // Header
  animate('.contact-header', {
    opacity: [0, 1],
    translateY: [30, 0],
    duration: 800,
    ease: 'easeOutExpo',
    autoplay: onScroll({
      target: '.contact-header',
      enter: 'bottom',
      leave: 'top',
    }),
  })

  // Header line
  animate('.contact-section .section-line', {
    scaleX: [0, 1],
    duration: 1000,
    ease: 'easeOutExpo',
    autoplay: onScroll({
      target: '.contact-section .section-line',
      enter: 'bottom',
      leave: 'top',
    }),
  })

  // Headline + desc
  animate('.contact-headline, .contact-desc', {
    opacity: [0, 1],
    translateY: [20, 0],
    duration: 700,
    delay: stagger(150),
    ease: 'easeOutExpo',
    autoplay: onScroll({
      target: '.contact-info',
      enter: 'bottom',
      leave: 'top',
    }),
  })

  // Contact links
  animate('.contact-link', {
    opacity: [0, 1],
    translateX: [-15, 0],
    duration: 500,
    delay: stagger(100),
    ease: 'easeOutExpo',
    autoplay: onScroll({
      target: '.contact-details',
      enter: 'bottom',
      leave: 'top',
    }),
  })

  // Form groups
  animate('.form-group, .form-submit', {
    opacity: [0, 1],
    translateY: [20, 0],
    duration: 600,
    delay: stagger(100),
    ease: 'easeOutExpo',
    autoplay: onScroll({
      target: '.contact-form',
      enter: 'bottom',
      leave: 'top',
    }),
  })

  // Footer line
  animate('.footer-line', {
    scaleX: [0, 1],
    duration: 1200,
    ease: 'easeOutExpo',
    autoplay: onScroll({
      target: '.site-footer',
      enter: 'bottom',
      leave: 'top',
    }),
  })

  // Footer content
  animate('.footer-content', {
    opacity: [0, 1],
    duration: 800,
    ease: 'easeOutExpo',
    autoplay: onScroll({
      target: '.site-footer',
      enter: 'bottom',
      leave: 'top',
    }),
  })
})
</script>

<style scoped>
/* ── Section header ────────────────────────────────────────────────────────── */
.contact-header {
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

/* ── Contact info ──────────────────────────────────────────────────────────── */
.contact-headline {
  font-family: 'Oxanium', sans-serif;
  font-size: 2.2rem;
  font-weight: 500;
  line-height: 1.2;
  color: rgba(240, 240, 255, 0.9);
  margin-bottom: 1.5rem;
  opacity: 0;
}

.headline-accent {
  color: #e63946;
}

.contact-desc {
  font-family: 'DM Mono', monospace;
  font-size: 0.8rem;
  line-height: 1.8;
  color: rgba(200, 200, 220, 0.5);
  margin-bottom: 2rem;
  max-width: 400px;
  opacity: 0;
}

/* ── Contact links ─────────────────────────────────────────────────────────── */
.contact-details {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.contact-link {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  font-family: 'DM Mono', monospace;
  font-size: 0.75rem;
  color: rgba(240, 240, 255, 0.5);
  text-decoration: none;
  letter-spacing: 0.03em;
  transition: color 0.2s ease;
  opacity: 0;
}

.contact-link:hover {
  color: #e63946;
}

.contact-link svg {
  color: rgba(230, 57, 70, 0.6);
  flex-shrink: 0;
}

/* ── Form ───────────────────────────────────────────────────────────────────── */
.contact-form {
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 6px;
  opacity: 0;
}

.form-label {
  font-family: 'DM Mono', monospace;
  font-size: 0.6rem;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  color: rgba(200, 200, 220, 0.4);
}

.form-input {
  background: rgba(255, 255, 255, 0.025);
  border: 1px solid rgba(255, 255, 255, 0.06);
  padding: 0.75rem 1rem;
  font-family: 'DM Mono', monospace;
  font-size: 0.8rem;
  color: rgba(240, 240, 255, 0.8);
  outline: none;
  transition: border-color 0.25s ease, box-shadow 0.25s ease;
}

.form-input::placeholder {
  color: rgba(200, 200, 220, 0.2);
}

.form-input:focus {
  border-color: rgba(230, 57, 70, 0.35);
  box-shadow: 0 0 0 2px rgba(230, 57, 70, 0.08);
}

.form-textarea {
  resize: vertical;
  min-height: 120px;
}

.form-submit {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  align-self: flex-start;
  padding: 0.65rem 1.5rem;
  background: transparent;
  border: 1px solid rgba(230, 57, 70, 0.4);
  font-family: 'DM Mono', monospace;
  font-size: 0.72rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: #e63946;
  cursor: pointer;
  position: relative;
  overflow: hidden;
  transition: background 0.25s ease, border-color 0.25s ease, box-shadow 0.25s ease;
  opacity: 0;
}

.form-submit:hover {
  background: rgba(230, 57, 70, 0.08);
  border-color: rgba(230, 57, 70, 0.7);
  box-shadow: 0 0 20px rgba(230, 57, 70, 0.15);
}

/* ── Footer ────────────────────────────────────────────────────────────────── */
.site-footer {
  margin-top: 6rem;
  padding-top: 2rem;
}

.footer-line {
  height: 1px;
  background: linear-gradient(90deg, transparent, rgba(230, 57, 70, 0.2), transparent);
  margin-bottom: 2rem;
  transform: scaleX(0);
}

.footer-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  opacity: 0;
}

@media (max-width: 768px) {
  .footer-content {
    flex-direction: column;
    gap: 0.5rem;
    text-align: center;
  }
}

.footer-copy {
  font-family: 'DM Mono', monospace;
  font-size: 0.65rem;
  color: rgba(200, 200, 220, 0.3);
  letter-spacing: 0.05em;
}

.footer-built {
  font-family: 'DM Mono', monospace;
  font-size: 0.6rem;
  color: rgba(200, 200, 220, 0.2);
  letter-spacing: 0.05em;
}
</style>
