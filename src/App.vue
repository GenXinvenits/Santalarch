<script setup>
import { onMounted, onUnmounted, ref } from 'vue'
import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'

gsap.registerPlugin(ScrollTrigger)

const menuOpen = ref(false)
const year = new Date().getFullYear()

const services = [
  { number: '01', title: 'Software & web', description: 'Thoughtful websites, web applications, and digital products built to feel clear, fast, and dependable.', tags: ['Web platforms', 'Frontend engineering', 'Product development'] },
  { number: '02', title: 'Systems & automation', description: 'Connect the moving parts. Reduce repetitive work and turn complicated workflows into reliable systems.', tags: ['Workflow design', 'Integrations', 'Automation'] },
  { number: '03', title: 'Technology consulting', description: 'Make better technical decisions with a practical plan, honest trade-offs, and a clear path forward.', tags: ['Architecture', 'Technical strategy', 'Problem solving'] },
  { number: '04', title: 'Design & experience', description: 'Translate complex ideas into intuitive interfaces, cohesive visual systems, and useful experiences.', tags: ['UI / UX', 'Design systems', 'Prototyping'] },
]

const steps = [
  { n: '01', title: 'Understand', text: 'We ask better questions before reaching for a solution.' },
  { n: '02', title: 'Untangle', text: 'We find the real problem, map the constraints, and simplify the moving parts.' },
  { n: '03', title: 'Build', text: 'We turn a considered plan into working, testable technology.' },
  { n: '04', title: 'Refine', text: 'We test the details, improve what matters, and leave things easier to maintain.' },
]

function closeMenu() {
  menuOpen.value = false
}

let ctx
onMounted(() => {
  ctx = gsap.context(() => {
    gsap.from('.hero-kicker, .hero-title, .hero-copy, .hero-actions', {
      y: 24, opacity: 0, duration: 0.8, stagger: 0.12, ease: 'power3.out', delay: 0.12
    })
    gsap.utils.toArray('[data-reveal]').forEach((el) => {
      gsap.from(el, {
        y: 28, opacity: 0, duration: 0.7, ease: 'power2.out',
        scrollTrigger: { trigger: el, start: 'top 86%', once: true }
      })
    })
  })
})
onUnmounted(() => ctx?.revert())
</script>

<template>
  <div class="site-shell">
    <header class="site-header">
      <a class="brand" href="#top" aria-label="Dibacii Technologies home" @click="closeMenu">
        <svg class="brand-mark" viewBox="0 0 42 42" fill="none" aria-hidden="true">
          <path d="M7 7H21C29 7 35 13 35 21S29 35 21 35H7V7Z" stroke="currentColor" stroke-width="2"/>
          <path d="M15 15H22C25.4 15 28 17.6 28 21S25.4 27 22 27H15V15Z" stroke="currentColor" stroke-width="2"/>
          <circle cx="21" cy="21" r="2.5" fill="currentColor"/>
        </svg>
        <span class="brand-wordmark">DIBACII<span>®</span><small>TECHNOLOGIES</small></span>
      </a>
      <button class="menu-toggle" type="button" :aria-expanded="menuOpen" aria-label="Toggle navigation" @click="menuOpen = !menuOpen">
        <span></span><span></span>
      </button>
      <nav class="main-nav" :class="{ 'is-open': menuOpen }" aria-label="Main navigation">
        <a href="#what-we-do" @click="closeMenu">What we do</a>
        <a href="#approach" @click="closeMenu">Our approach</a>
        <a href="#about" @click="closeMenu">About</a>
        <a class="nav-contact" href="#contact" @click="closeMenu">Start a conversation <span>↗</span></a>
      </nav>
    </header>

    <main id="top">
      <section class="hero section-pad">
        <div class="hero-main">
          <p class="eyebrow hero-kicker"><span class="status-dot"></span> INDEPENDENT TECHNOLOGY STUDIO <span class="eyebrow-index">[ 01 — 04 ]</span></p>
          <h1 class="hero-title">The art of<br>figuring <span class="title-accent">things out.</span></h1>
          <p class="hero-copy">Dibacii Technologies turns complex problems into clear, considered digital solutions. Strategy, engineering, and design—working as one.</p>
          <div class="hero-actions">
            <a class="button button-primary" href="#what-we-do">Explore what we do <span>↘</span></a>
            <a class="text-link" href="#approach">How we think <span>↗</span></a>
          </div>
        </div>
        <div class="hero-art" aria-label="Abstract geometric illustration representing connected systems">
          <div class="art-topline"><span>FIG. 001</span><span>IDEAS INTO SYSTEMS</span></div>
          <div class="art-grid"></div>
          <svg class="orbit-art" viewBox="0 0 620 560" fill="none" aria-hidden="true">
            <defs>
              <linearGradient id="orbGlow" x1="88" y1="95" x2="516" y2="455" gradientUnits="userSpaceOnUse">
                <stop stop-color="#d9ff68"/><stop offset="1" stop-color="#9bd83f" stop-opacity=".1"/>
              </linearGradient>
              <radialGradient id="orbFill" cx="0" cy="0" r="1" gradientTransform="matrix(195 184 -175 185 302 269)" gradientUnits="userSpaceOnUse">
                <stop stop-color="#d9ff68" stop-opacity=".22"/><stop offset="1" stop-color="#d9ff68" stop-opacity="0"/>
              </radialGradient>
            </defs>
            <circle cx="310" cy="280" r="210" fill="url(#orbFill)"/>
            <ellipse cx="310" cy="280" rx="210" ry="83" transform="rotate(-35 310 280)" stroke="currentColor" stroke-opacity=".45"/>
            <ellipse cx="310" cy="280" rx="210" ry="83" transform="rotate(35 310 280)" stroke="currentColor" stroke-opacity=".45"/>
            <ellipse cx="310" cy="280" rx="112" ry="210" stroke="currentColor" stroke-opacity=".3"/>
            <circle cx="310" cy="280" r="150" stroke="currentColor" stroke-opacity=".18"/>
            <path d="M106 390 218 325 310 280 420 160 520 206" stroke="url(#orbGlow)" stroke-width="1.5"/>
            <path d="M145 145 235 230 310 280 390 345 466 421" stroke="url(#orbGlow)" stroke-width="1.5"/>
            <path d="M310 70V490M100 280H520" stroke="currentColor" stroke-opacity=".2" stroke-dasharray="3 7"/>
            <path d="M240 210 310 170 380 210V290L310 330 240 290V210Z" stroke="#d9ff68" stroke-width="2"/>
            <path d="M240 210 310 250 380 210M310 250V330" stroke="#d9ff68" stroke-width="2"/>
            <path d="M240 290 310 250 380 290" stroke="#d9ff68" stroke-opacity=".65"/>
            <circle cx="310" cy="250" r="5" fill="#d9ff68"/>
            <circle cx="420" cy="160" r="4" fill="#d9ff68"/>
            <circle cx="145" cy="145" r="4" fill="#d9ff68"/>
            <circle cx="466" cy="421" r="4" fill="#d9ff68"/>
            <circle cx="106" cy="390" r="4" fill="#d9ff68"/>
          </svg>
          <div class="art-coordinate coordinate-one">31° / 75°<small>FIND THE SIGNAL</small></div>
          <div class="art-coordinate coordinate-two">SYSTEMS / 001<small>MAKE IT MAKE SENSE</small></div>
          <span class="art-cross cross-one">+</span><span class="art-cross cross-two">+</span>
          <div class="art-caption"><span>01</span><span>THINK CLEARLY. BUILD WITH INTENT.</span></div>
        </div>
        <div class="hero-bottom"><span>INQUISITIVE BY NATURE.</span><span>PRECISE BY DESIGN.</span><span class="scroll-cue">SCROLL TO EXPLORE ↓</span></div>
      </section>

      <section class="intro-band section-pad" data-reveal>
        <p class="eyebrow">A DIFFERENT KIND OF TECHNOLOGY PARTNER</p>
        <p class="intro-statement">Good technology isn't about making things <em>complicated.</em> It's about making the complicated <span>work beautifully.</span></p>
        <div class="intro-foot"><span>CURIOUS MINDS. PRACTICAL THINKING.</span><span>BUILT AROUND THE PROBLEM, NOT THE HYPE.</span></div>
      </section>

      <section id="what-we-do" class="services-section section-pad">
        <div class="section-heading" data-reveal>
          <div><p class="eyebrow">01 / CAPABILITIES</p><h2>Make the difficult<br><span>feel simple.</span></h2></div>
          <p class="section-summary">From the first question to the final detail, we bring the right mix of thinking and making to every challenge.</p>
        </div>
        <div class="service-list">
          <article v-for="service in services" :key="service.number" class="service-row" data-reveal>
            <span class="service-number">{{ service.number }}</span>
            <div class="service-main"><h3>{{ service.title }}</h3><p>{{ service.description }}</p><div class="service-tags"><span v-for="tag in service.tags" :key="tag">{{ tag }}</span></div></div>
            <a class="service-arrow" href="#contact" :aria-label="'Discuss ' + service.title">↗</a>
          </article>
        </div>
        <div class="capability-note"><span class="status-dot"></span> ONE TEAM. CONNECTED DISCIPLINES. <span class="capability-note-right">NO OFF-THE-SHELF THINKING.</span></div>
      </section>

      <section id="approach" class="approach-section section-pad">
        <div class="approach-head" data-reveal>
          <p class="eyebrow">02 / HOW WE WORK</p>
          <h2>Curiosity is the<br><span>starting point.</span></h2>
          <p>Every worthwhile solution begins with a better question. We look beneath the brief, challenge assumptions, and make a plan that fits the real world.</p>
        </div>
        <div class="process-grid">
          <article v-for="step in steps" :key="step.n" class="process-card" data-reveal>
            <div class="process-card-top"><span>{{ step.n }}</span><svg viewBox="0 0 44 44" fill="none" aria-hidden="true"><circle cx="22" cy="22" r="17" stroke="currentColor" stroke-opacity=".35"/><path d="M10 28 20 18 27 25 35 14M27 14H35V22" stroke="currentColor" stroke-width="1.5"/></svg></div>
            <h3>{{ step.title }}</h3><p>{{ step.text }}</p>
          </article>
        </div>
        <div class="approach-bottom"><span>THOUGHTFUL BY DEFAULT.</span><span>ALWAYS LEARNING.</span><span>BUILT TO LAST.</span></div>
      </section>

      <section id="about" class="manifesto-section section-pad">
        <div class="manifesto-label" data-reveal><p class="eyebrow">03 / THE DIBACII MINDSET</p><span>SKILL ISN'T A SHORTCUT.<br>IT'S THE WORK.</span></div>
        <div class="manifesto-content" data-reveal>
          <h2>We believe the best tool is a <span>well-understood problem.</span></h2>
          <p>Dibacii is rooted in the idea of ingenuity: the patience to investigate, the craft to make, and the judgement to know what matters. We bring that mindset to technology—staying curious, sweating the details, and keeping the human on the other side in view.</p>
          <div class="manifesto-signature"><span class="signature-mark">D.</span><span>THE ART OF FIGURING THINGS OUT.<small>THE IDEA BEHIND DIBACII.</small></span></div>
        </div>
        <div class="manifesto-graphic" aria-hidden="true"><span class="graphic-ring ring-a"></span><span class="graphic-ring ring-b"></span><span class="graphic-ring ring-c"></span><span class="graphic-core">D</span><span class="graphic-label">INGENUITY<br>IN MOTION</span></div>
      </section>

      <section class="closing-quote section-pad" data-reveal>
        <span class="quote-mark">“</span><p>There is always a clearer way<br>to <em>figure it out.</em></p><span class="quote-index">DIBACII / PRINCIPLE 001</span>
      </section>

      <section id="contact" class="contact-section section-pad">
        <div class="contact-top" data-reveal><p class="eyebrow">04 / YOUR TURN</p><span>HAVE A GOOD PROBLEM?</span></div>
        <div class="contact-main" data-reveal><h2>Let's figure<br><span>it out.</span></h2><a class="contact-orb" href="mailto:hello@dibacii.com" aria-label="Email Dibacii Technologies"><span>START A<br>CONVERSATION</span><b>↗</b></a></div>
        <div class="contact-foot"><p>Tell us what you're building, fixing, or trying to make sense of.</p><a href="mailto:hello@dibacii.com">hello@dibacii.com <span>↗</span></a></div>
      </section>
    </main>

    <footer class="site-footer section-pad">
      <a class="brand footer-brand" href="#top">
        <svg class="brand-mark" viewBox="0 0 42 42" fill="none" aria-hidden="true"><path d="M7 7H21C29 7 35 13 35 21S29 35 21 35H7V7Z" stroke="currentColor" stroke-width="2"/><path d="M15 15H22C25.4 15 28 17.6 28 21S25.4 27 22 27H15V15Z" stroke="currentColor" stroke-width="2"/><circle cx="21" cy="21" r="2.5" fill="currentColor"/></svg>
        <span class="brand-wordmark">DIBACII<small>TECHNOLOGIES</small></span>
      </a>
      <p>Independent thinking. Useful technology.</p>
      <div class="footer-meta"><span>© {{ year }} DIBACII TECHNOLOGIES</span><a href="#top">BACK TO TOP ↑</a><span>MADE WITH INTENT.</span></div>
    </footer>
  </div>
</template>