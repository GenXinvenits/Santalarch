<script setup>
import { onMounted, onUnmounted, ref } from 'vue'
import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'
import DoorStudy from './components/DoorStudy.vue'
import CubePassage from './components/CubePassage.vue'

gsap.registerPlugin(ScrollTrigger)

const menuOpen = ref(false)
const cookieVisible = ref(true)
const activeService = ref(0)
const infrastructureOpen = ref(false)
const visibleProjects = ref(6)

const services = [
  { title: 'Architecture', copy: 'Projects from private residences to large masterplans across Europe and the Middle East.' },
  { title: 'Interior', copy: 'Spatial identity shaped into a coherent, memorable human experience.' },
  { title: 'Procurement', copy: 'Tenders, suppliers, and production coordinated for predictable delivery.' },
  { title: 'Consulting', copy: 'Site potential, brief, and strategy clarified before design begins.' },
  { title: 'Marketing', copy: 'Positioning, narrative, and presentation shaping market perception.' },
]

const stages = [
  'Strategic Consulting',
  'Alignment Session',
  'Design Foundation',
  'Project Development',
  'Delivery Support',
  'Market Positioning',
]
const scope = [
  'Master plan or layout options',
  'Functional zoning',
  'Architectural and interior concept',
  'Spatial organisation',
  'Material direction',
  'Lighting and furniture references',
  'Conceptual visualisations',
]
const projects = ['01', '02', '03', '04', '05', '06', '07', '08', '09', '10', '11', '12']
const team = [
  { name: 'Mariia Plyuta', role: 'Lead Architect (Berlin)' },
  { name: 'Kyrylo Bastrychev', role: 'Founder & Architect' },
  { name: 'Misak Melkonean', role: 'Lead Architect (London)' },
]
const countries = ['France', 'Germany', 'Hungary', 'Ukraine', 'UAE', 'Spain', 'Switzerland', 'Italy', 'Monaco', 'UK', 'Singapore', 'KSA']

function closeMenu() { menuOpen.value = false }

let animationContext
onMounted(() => {
  animationContext = gsap.context(() => {
    gsap.from('.hero-copy > *', {
      y: 22, opacity: 0, duration: 1, stagger: 0.12, ease: 'power3.out',
      delay: 0.15,
    })
    gsap.utils.toArray('[data-reveal]').forEach((element) => {
      gsap.from(element, {
        y: 30, opacity: 0, duration: 0.8, ease: 'power2.out',
        scrollTrigger: { trigger: element, start: 'top 88%', once: true },
      })
    })
  })
})
onUnmounted(() => animationContext?.revert())
</script>

<template>
  <div class="site-shell">
    <div v-if="cookieVisible" class="cookie-note" aria-label="Cookie preferences">
      <span>This site uses cookies.</span>
      <div class="cookie-actions">
        <button @click="cookieVisible = false">Decline</button>
        <button class="cookie-accept" @click="cookieVisible = false">Accept</button>
      </div>
    </div>

    <header class="site-header">
      <a class="wordmark" href="#top" aria-label="SANTAL home">SANTAL<span>®</span></a>
      <nav class="desktop-nav" aria-label="Main navigation">
        <a href="#philosophy">Philosophy</a><a href="#works">Works</a><a href="#services">Services</a>
        <a href="#process">Process</a><a href="#company">Company</a><a href="#contact">Contact</a>
      </nav>
      <button class="menu-toggle" :aria-expanded="menuOpen" @click="menuOpen = !menuOpen">
        <span>{{ menuOpen ? 'Close' : 'Menu' }}</span><i :class="{ open: menuOpen }"><b></b><b></b></i>
      </button>
    </header>

    <div v-if="menuOpen" class="menu-overlay">
      <a v-for="item in [['Philosophy','#philosophy'],['Works','#works'],['Services','#services'],['Process','#process'],['Company','#company'],['Contact','#contact']]" :key="item[0]" :href="item[1]" @click="closeMenu">{{ item[0] }}</a>
      <span class="menu-index">SANTAL SA · LUGANO · WORLDWIDE</span>
    </div>

    <main id="top">
      <section class="hero section-pad">
        <div class="hero-meta"><span>ARCHITECTURE / INTERIOR / PROCUREMENT</span><span>46°00' N — 8°57' E</span></div>
        <div class="hero-copy">
          <p class="eyebrow">Architectural studio</p>
          <h1>built on uncompromising<br /><em>precision and intent.</em></h1>
          <div class="hero-bottom"><p>Architecture Shapes Perception</p><p>Bold presence.<br />Quiet execution.</p></div>
        </div>
        <div class="hero-visual" aria-label="Architectural line drawing">
          <div class="blueprint-grid"></div>
          <svg class="hero-drawing" viewBox="0 0 900 580" role="img" aria-label="Abstract architectural elevation drawing">
            <g fill="none" stroke="currentColor" stroke-width="1">
              <path d="M75 480 L75 180 L255 72 L440 180 L440 480 Z M75 180 L440 180 M255 72 L255 480 M75 330 L440 330"/>
              <path d="M440 480 L440 225 L650 110 L825 215 L825 480 Z M440 225 L825 215 M650 110 L650 480 M440 355 L825 355"/>
              <path d="M110 215 H220 V290 H110 Z M290 215 H405 V290 H290 Z M110 365 H220 V450 H110 Z M290 365 H405 V450 H290 Z"/>
              <path d="M475 260 H610 V330 H475 Z M680 260 H790 V330 H680 Z M475 385 H610 V455 H475 Z M680 385 H790 V455 H680 Z"/>
              <path d="M35 500 H860 M255 45 V520 M55 180 H455 M650 85 V520" stroke-dasharray="4 6" opacity=".5"/>
              <circle cx="255" cy="180" r="7"/><circle cx="650" cy="225" r="7"/>
            </g>
          </svg>
          <span class="drawing-label label-a">FIG. 001 / ELEVATION</span><span class="drawing-label label-b">S — 01:100</span>
          <span class="drawing-coordinates">X 04 / Y 12 / Z 08</span>
        </div>
        <a href="#philosophy" class="scroll-cue"><span class="scroll-line"></span><span>Scroll to discover</span><span>↓</span></a>
        <div class="hero-index">S / 001</div>
      </section>

      <section id="philosophy" class="philosophy section-pad">
        <div class="section-kicker"><span>01 / PHILOSOPHY</span><span>Our position</span></div>
        <div class="philosophy-grid">
          <article data-reveal><span class="number">01</span><h2>Bold<span>.</span></h2><p>A clear visual position, formed through strength, precision, and emotional gravity — a presence that stays beneath the surface.</p></article>
          <article data-reveal><span class="number">02</span><h2>Confident<span>.</span></h2><p>A precise system of decisions, where every detail is resolved to create reliability, clarity, and lasting performance.</p></article>
          <article data-reveal><span class="number">03</span><h2>Quiet<span>.</span></h2><p>A quiet presence, formed through restraint and balance — architecture that does not shout, yet continues to resonate.</p></article>
        </div>
      </section>

      <section class="vision section-pad">
        <div class="section-kicker"><span>02 / OUR VISION</span><span>Direction becomes form</span></div>
        <div class="vision-title"><span class="giant-word">WE</span><div><p>Our Vision</p><h2>Architecture is not an image.<br /><em>It is a system of intent.</em></h2></div></div>
        <div class="vision-foot"><span>00 — 03</span><p>We bring together perception, engineering, and material intelligence to create architecture with a lasting point of view.</p></div>
      </section>

      <section class="approach section-pad">
        <div class="section-kicker"><span>03 / OUR APPROACH</span><span>See · Hear · Touch · Feel</span></div>
        <div class="approach-intro"><p class="eyebrow">A considered process</p><h2>Inspiration defines direction —<br /><em>engineering secures it.</em></h2><p>We translate vision into architecture through a structured development process.</p></div>
        <div class="principles">
          <article data-reveal><span>01 / INTENT</span><h3>Intent Before Image</h3><p>Every project begins with a clear architectural direction defined through proportion, hierarchy, and spatial logic. Form must remain coherent under structural and technical evaluation.</p></article>
          <article data-reveal><span>02 / STRUCTURE</span><h3>Engineering<br />as Foundation</h3><p>Structural systems, tolerances, and interdisciplinary coordination are resolved early. Architecture must be buildable, rational, and exact — not merely expressive.</p></article>
          <article data-reveal><span>03 / MATERIAL</span><h3>Material Intelligence</h3><p>Materials are selected for performance, longevity, tactility, and light interaction. Authenticity is essential. Every surface must justify its presence structurally and atmospherically.</p></article>
          <article data-reveal><span>04 / DETAIL</span><h3>Detail<br />as Discipline</h3><p>Junctions, transitions, alignments, and finishing tolerances are refined through multiple development stages. Ambiguity is eliminated before construction begins.</p></article>
        </div>
        <DoorStudy />
      </section>

      <section id="works" class="works section-pad">
        <div class="section-kicker"><span>04 / SELECTED WORK</span><span>Portfolio / 2020—2026</span></div>
        <div class="section-heading"><h2>Our Featured<br /><em>Projects</em></h2><span class="heading-aside">A selection of work<br />in progress and realised.</span></div>
        <div class="project-grid">
          <a v-for="(project, i) in projects.slice(0, visibleProjects)" :key="project" href="#contact" class="project-card" :class="'project-card-' + (i % 6 + 1)">
            <div class="project-art"><div class="project-art-grid"></div><div class="project-shape"></div><span class="project-figure">S / {{ project }}</span><span class="project-open">↗</span></div>
            <div class="project-caption"><span>Project / {{ project }}</span><span>Architecture · Interior</span></div>
          </a>
        </div>
        <button v-if="visibleProjects < projects.length" class="text-button load-more" @click="visibleProjects = Math.min(visibleProjects + 6, projects.length)">Load more <span>＋</span></button>
      </section>

      <CubePassage />

      <section id="services" class="services section-pad">
        <div class="section-kicker"><span>05 / WHAT WE DO</span><span>Integrated disciplines</span></div>
        <div class="service-layout"><div class="service-list"><button v-for="(service, i) in services" :key="service.title" :class="{ active: activeService === i }" @mouseenter="activeService = i" @focus="activeService = i" @click="activeService = i"><span>0{{ i + 1 }}</span><span>{{ service.title }}</span><span>↗</span></button></div><div class="service-detail" :key="activeService"><span class="service-index">DISCIPLINE / 0{{ activeService + 1 }}</span><h3>{{ services[activeService].title }}</h3><p>{{ services[activeService].copy }}</p><a href="#contact">Discuss a project <span>↗</span></a></div></div>
      </section>

      <section id="process" class="process section-pad">
        <div class="section-kicker"><span>06 / OUR PROCESS</span><span>From intent to delivery</span></div>
        <div class="section-heading"><h2>Every project is a sequence<br />of <em>clear decisions.</em></h2><p>Each stage establishes the conditions for the next — from early strategy to the final detail.</p></div>
        <div class="stage-list"><details v-for="(stage, i) in stages" :key="stage" :open="i === 0"><summary><span>0{{ i + 1 }}</span><h3>{{ stage }}</h3><span class="stage-toggle">＋</span></summary><div class="stage-content"><p>We establish the spatial, architectural, and visual direction of the project. Planning options, functional relationships, circulation, proportions, material language, atmosphere, and key design principles are developed into a coherent concept.</p><div><span>Scope</span><ul><li v-for="item in scope" :key="item">{{ item }}</li></ul></div></div></details></div>
      </section>

      <section id="company" class="company section-pad">
        <div class="section-kicker"><span>07 / COMPANY</span><span>People · Places · Perspective</span></div>
        <div class="company-intro"><h2>Working<br /><em>Worldwide.</em></h2><p>Our team is united by attention to detail, high standards, and uncompromising expectations of results.</p></div>
        <div class="country-line"><span v-for="country in countries" :key="country">{{ country }}</span></div>
        <div class="team-grid"><article v-for="(person, i) in team" :key="person.name"><div class="team-portrait"><span>0{{ i + 1 }}</span><div class="portrait-mark">{{ person.name.split(' ').map(n => n[0]).join('') }}</div></div><div class="team-name"><h3>{{ person.name }}</h3><p>{{ person.role }}</p></div></article></div>
        <div class="worldwide-note"><span>INTERNATIONAL PRACTICE</span><p>Our team works on international projects and procurement operations all over the world.</p><span>46°00' N / 8°57' E</span></div>
      </section>

      <section id="contact" class="contact section-pad">
        <div class="section-kicker"><span>08 / CONTACT</span><span>Start a conversation</span></div>
        <p class="eyebrow">Have a project in mind?</p><h2>Let’s <em>talk.</em></h2>
        <div class="contact-grid"><div><span class="contact-label">Enquire</span><a href="mailto:info@santalarch.com">info@santalarch.com <span>↗</span></a><a href="mailto:partnerships@santalarch.com">partnerships@santalarch.com <span>↗</span></a><a href="mailto:careers@santalarch.com">careers@santalarch.com <span>↗</span></a></div><div><span class="contact-label">Address</span><p>Via Prati Botta 6<br />6917 Lugano, TI, Switzerland</p><a href="https://maps.google.com/?q=Via+Prati+Botta+6+Lugano" target="_blank" rel="noreferrer">Open in Maps <span>↗</span></a></div></div>
      </section>

      <section class="infrastructure section-pad">
        <div class="section-kicker"><span>09 / DIGITAL INFRASTRUCTURE</span><span>Nothing is secondary</span></div>
        <div class="infrastructure-copy"><h2>Nothing<br /><em>is secondary.</em></h2><div><p>We believe that the quality of a project is defined as much by the decisions that remain unseen as by those that are immediately visible.</p><p>The same principle extends to our own digital infrastructure. SANTAL’s production website is hosted in Switzerland within Flow Swiss’ ALP2 region — an underground data centre built inside a former civil-defence bunker in Lucerne.</p><div v-if="infrastructureOpen"><p>Its infrastructure combines physical resilience with responsible energy use: the facility operates on renewable energy, uses water from Lake Lucerne for cooling, and returns recovered server heat to the local district heating network.</p><p>For us, this is not an isolated technical decision. It reflects a broader principle: security, longevity, efficiency and responsible use of resources should be considered at every scale.</p></div><button class="text-button" @click="infrastructureOpen = !infrastructureOpen">{{ infrastructureOpen ? 'Read Less' : 'Read More' }} <span>{{ infrastructureOpen ? '−' : '+' }}</span></button></div></div>
      </section>
    </main>

    <footer class="site-footer section-pad">
      <div class="footer-top"><a class="wordmark footer-mark" href="#top">SANTAL<span>®</span></a><a href="#top" class="back-top">Back to top ↑</a></div>
      <div class="footer-bottom"><span>Copyright © 2026 SANTAL SA — All Rights Reserved.</span><div><a href="#contact">Privacy Policy</a><a href="#contact">Terms of Use</a></div><span>Architecture Shapes Perception</span></div>
    </footer>
  </div>
</template>