<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount } from 'vue'
import AOS from 'aos'
import 'aos/dist/aos.css'

import profilePic from '@/assets/lucy.jpeg' // add Lucy's photo here
import logo from '@/assets/lucy-logo.png' // optional logo; remove the <img> tags if unused

/* ---------------- CONFIG ---------------- */
const whatsappNumber = '2340000000000' // replace with Lucy's number
const openWhatsApp = (message = '') =>
  window.open(
    `https://wa.me/${whatsappNumber}${message ? `?text=${encodeURIComponent(message)}` : ''}`,
    '_blank'
  )

/* ---------------- CONTENT (replace placeholders) ---------------- */
const stats = [
  { label: 'Clients supported', value: 800, suffix: '+' },
  { label: 'Years of practice', value: 1, suffix: '' },
  { label: 'Therapy hours', value: 9500, suffix: '+' },
  { label: 'Client satisfaction', value: 98, suffix: '%' }
]
const statValues = reactive(stats.map(() => 0))
const statsEl = ref(null)

const services = [
  { icon: 'mdi-account-heart-outline', title: 'Individual therapy', copy: 'One-to-one sessions to work through anxiety, low mood, trauma, grief and life transitions at your own pace.' },
  { icon: 'mdi-heart-multiple-outline', title: 'Couples & family', copy: 'Rebuild communication, trust and understanding with structured, judgment-free sessions.' },
  { icon: 'mdi-brain', title: 'CBT & mindfulness', copy: 'Evidence-based tools to change unhelpful thought patterns and build lasting emotional resilience.' },
  { icon: 'mdi-briefcase-check-outline', title: 'Workplace wellbeing', copy: 'Burnout recovery, stress management and wellbeing workshops for professionals and teams.' }
]

const approach = [
  { icon: 'mdi-chat-processing-outline', title: 'Free intro call', copy: 'A 15-minute chat to share what brings you here and see if we are a good fit.' },
  { icon: 'mdi-clipboard-text-outline', title: 'Assessment', copy: 'We map your history, goals and what support looks like for you.' },
  { icon: 'mdi-sprout-outline', title: 'Tailored plan', copy: 'A clear, flexible plan with measurable goals, reviewed together regularly.' },
  { icon: 'mdi-trending-up', title: 'Progress & growth', copy: 'Regular check-ins so you can see how far you have come and what is next.' }
]

const specialties = ['Anxiety', 'Depression', 'Trauma & PTSD', 'Grief', 'Burnout', 'Relationships', 'Self-esteem', 'Life transitions', 'Stress', 'Sleep issues']

const credentials = [
  { icon: 'mdi-school-outline', title: 'Ph.D. Clinical Psychology', copy: 'Replace with university and year' },
  { icon: 'mdi-certificate-outline', title: 'Licensed Psychologist', copy: 'Replace with licence body and number' },
  { icon: 'mdi-shield-check-outline', title: 'Confidential & ethical', copy: 'Practice follows professional ethics and privacy standards' }
]

const resources = [
  { tag: 'Anxiety', title: '5 grounding techniques you can use anywhere', time: '4 min read' },
  { tag: 'Wellbeing', title: 'How to build a sleep routine your mind will thank you for', time: '6 min read' },
  { tag: 'Relationships', title: 'Healthy boundaries: what they are and how to set them', time: '5 min read' }
]

const testimonials = [
  { quote: 'Lucy created a space where I felt heard for the first time. Within weeks I had practical tools for my anxiety and a calmer mind.', name: 'Client A.', role: 'Individual therapy' },
  { quote: 'Our communication changed completely. We came in stuck and left with a way to talk to each other again.', name: 'Client B.', role: 'Couples therapy' },
  { quote: 'Professional, warm and never judgmental. The burnout recovery plan gave me my energy and my weekends back.', name: 'Client C.', role: 'Workplace wellbeing' }
]
const activeT = ref(0)
let tTimer
const pickT = (i) => { activeT.value = i; restartT() }
const restartT = () => { clearInterval(tTimer); tTimer = setInterval(() => (activeT.value = (activeT.value + 1) % testimonials.length), 6000) }

const plans = [
  { name: 'Intro call', price: 'Free', period: '15 minutes', tag: null, features: ['Share what you need', 'Ask any questions', 'No commitment'], cta: 'Book intro call' },
  { name: 'Therapy session', price: 'Custom', period: 'per session', tag: 'Most popular', features: ['50-minute session', 'Online or in person', 'Between-session resources', 'Progress reviews'], cta: 'Book a session' },
  { name: 'Care package', price: 'Custom', period: 'per month', tag: null, features: ['4 sessions a month', 'Priority scheduling', 'Message support', 'Personal workbook'], cta: 'Get a package' }
]

const faqs = [
  { q: 'How do I know if therapy is right for me?', a: 'If something feels heavy, stuck or confusing, therapy can help. The free intro call is a low-pressure way to find out.' },
  { q: 'Are sessions confidential?', a: 'Yes. What you share stays private, within the limits of professional and legal ethics, which we cover in the first session.' },
  { q: 'Do you offer online sessions?', a: 'Yes. Sessions are available by secure video call as well as in person.' },
  { q: 'How many sessions will I need?', a: 'It depends on your goals. Some people feel better in a few weeks; others prefer longer-term support. We review this together.' },
  { q: 'What if I need urgent help?', a: 'Therapy is not an emergency service. If you are in immediate danger, contact your local emergency number or a crisis line right away.' }
]
const openFaq = ref(0)

const footerLinks = {
  Practice: ['About', 'Services', 'Approach', 'Pricing'],
  Connect: ['Email', 'WhatsApp', 'Instagram', 'LinkedIn'],
  Resources: ['Articles', 'Breathing tool', 'FAQ', 'Book a session']
}

/* ---------------- MOOD CHECK-IN ---------------- */
const moods = [
  { icon: 'mdi-emoticon-sad-outline', label: 'Struggling', msg: 'Thank you for being honest. You do not have to carry this alone. Let us talk.' },
  { icon: 'mdi-emoticon-neutral-outline', label: 'Low', msg: 'Low days matter too. Support can help you feel lighter sooner.' },
  { icon: 'mdi-emoticon-outline', label: 'Okay', msg: 'Steady is a good place to build from. Therapy can help you grow from here.' },
  { icon: 'mdi-emoticon-happy-outline', label: 'Good', msg: 'Great to hear. Keep up what works, and reach out any time you want to go deeper.' }
]
const mood = ref(null)

/* ---------------- BREATHING TOOL ---------------- */
const phases = [
  { name: 'Breathe in', secs: 4, scale: 1.25 },
  { name: 'Hold', secs: 4, scale: 1.25 },
  { name: 'Breathe out', secs: 6, scale: 0.8 }
]
const breathing = ref(false)
const phaseIdx = ref(2)
const countdown = ref(0)
let bTimer
const toggleBreathing = () => {
  if (breathing.value) {
    clearInterval(bTimer); breathing.value = false; phaseIdx.value = 2; return
  }
  breathing.value = true; phaseIdx.value = 0; countdown.value = phases[0].secs
  bTimer = setInterval(() => {
    countdown.value--
    if (countdown.value <= 0) {
      phaseIdx.value = (phaseIdx.value + 1) % phases.length
      countdown.value = phases[phaseIdx.value].secs
    }
  }, 1000)
}
const circleStyle = computed(() => ({
  transform: `scale(${breathing.value ? phases[phaseIdx.value].scale : 1})`,
  transitionDuration: `${breathing.value ? phases[phaseIdx.value].secs : 0.6}s`
}))

/* ---------------- BOOKING CALENDAR ---------------- */
const workDays = [1, 2, 3, 4, 5]
const bookedDates = ['2026-11-12', '2026-11-18'] // YYYY-MM-DD
const slots = ['09:00', '10:30', '12:00', '14:00', '15:30', '17:00']
const sessionTypes = ['Online', 'In person']
const sessionType = ref('Online')
const now = new Date()
const todayStart = new Date(now.getFullYear(), now.getMonth(), now.getDate())
const viewDate = ref(new Date(now.getFullYear(), now.getMonth(), 1))
const selectedDate = ref(null)
const selectedSlot = ref(null)

const monthLabel = computed(() => viewDate.value.toLocaleDateString('en-GB', { month: 'long', year: 'numeric' }))
const canGoBack = computed(() => viewDate.value > new Date(now.getFullYear(), now.getMonth(), 1))
const changeMonth = (n) => { const d = viewDate.value; viewDate.value = new Date(d.getFullYear(), d.getMonth() + n, 1) }
const calendarCells = computed(() => {
  const y = viewDate.value.getFullYear(), m = viewDate.value.getMonth()
  const cells = Array(new Date(y, m, 1).getDay()).fill(null)
  for (let d = 1; d <= new Date(y, m + 1, 0).getDate(); d++) {
    const date = new Date(y, m, d)
    const key = `${y}-${String(m + 1).padStart(2, '0')}-${String(d).padStart(2, '0')}`
    const booked = bookedDates.includes(key)
    cells.push({ d, key, booked, available: date >= todayStart && workDays.includes(date.getDay()) && !booked })
  }
  return cells
})
const pickDate = (c) => { selectedDate.value = c.key; selectedSlot.value = null }
const confirmBooking = () =>
  openWhatsApp(`Hi Lucy, I'd like to book a ${sessionType.value.toLowerCase()} session on ${selectedDate.value} at ${selectedSlot.value}. Is that available?`)

/* ---------------- NAV / SCROLL ---------------- */
const scrollY = ref(0)
const progress = ref(0)
const mobileMenu = ref(false)
const onScroll = () => {
  scrollY.value = window.scrollY
  const h = document.documentElement.scrollHeight - window.innerHeight
  progress.value = h > 0 ? (window.scrollY / h) * 100 : 0
}
const scrollToId = (id) => { mobileMenu.value = false; document.getElementById(id)?.scrollIntoView({ behavior: 'smooth' }) }
const toTop = () => window.scrollTo({ top: 0, behavior: 'smooth' })

/* ---------------- COUNT-UP ---------------- */
let observer
const animateStats = () => {
  const start = performance.now(), dur = 1600
  const tick = (t) => {
    const p = Math.min((t - start) / dur, 1), e = 1 - Math.pow(1 - p, 3)
    stats.forEach((s, i) => (statValues[i] = Math.round(s.value * e)))
    if (p < 1) requestAnimationFrame(tick)
  }
  requestAnimationFrame(tick)
}

onMounted(() => {
  AOS.init({ duration: 800, once: true, easing: 'ease-out-cubic', offset: 80 })
  window.addEventListener('scroll', onScroll, { passive: true })
  restartT()
  observer = new IntersectionObserver(([e]) => { if (e.isIntersecting) { animateStats(); observer.disconnect() } }, { threshold: 0.4 })
  if (statsEl.value) observer.observe(statsEl.value)
})
onBeforeUnmount(() => {
  window.removeEventListener('scroll', onScroll)
  clearInterval(tTimer); clearInterval(bTimer); observer?.disconnect()
})
</script>

<template>
  <div class="if-shell w-full bg-white text-[#1E0F2E] overflow-x-hidden">
    <!-- scroll progress -->
    <div class="fixed top-0 left-0 z-50 h-[3px] transition-[width] duration-100" :style="{ width: progress + '%', background: 'linear-gradient(90deg,#5B1A8F,#D4147E)' }"></div>

    <!-- ============ NAVBAR ============ -->
    <header class="sticky top-0 z-40 bg-white/90 backdrop-blur border-b border-[#EDE4F5] transition-shadow" :class="scrollY > 20 ? 'shadow-sm' : ''">
      <div class="max-w-7xl mx-auto px-6 lg:px-10 h-16 flex items-center justify-between">
        <button class="flex items-center gap-2" @click="toTop">
          <img :src="logo" alt="Lucy Kim, Psychotherapist & Counsellor" class="h-11 sm:h-12 w-auto object-contain" />
        </button>

        <nav class="hidden md:flex items-center gap-8 text-[14px] text-[#6B5E78]">
          <a href="#services" class="hover:text-[#1E0F2E] transition-colors">Services</a>
          <a href="#approach" class="hover:text-[#1E0F2E] transition-colors">Approach</a>
          <a href="#calm" class="hover:text-[#1E0F2E] transition-colors">Calm space</a>
          <a href="#faq" class="hover:text-[#1E0F2E] transition-colors">FAQ</a>
          <a href="#book" class="hover:text-[#1E0F2E] transition-colors">Book</a>
        </nav>

        <div class="flex items-center gap-3">
          <button @click="openWhatsApp('Hi Lucy, I would like to get in touch with you')" class="hidden sm:block bg-[#5B1A8F] hover:bg-[#46107A] transition-colors text-white text-[14px] font-semibold px-4 py-2.5 rounded-full">
            Get in touch
          </button>
          <button class="md:hidden h-10 w-10 rounded-full hover:bg-[#F6F0FB]" @click="mobileMenu = !mobileMenu" aria-label="Menu">
            <i class="mdi text-[22px]" :class="mobileMenu ? 'mdi-close' : 'mdi-menu'"></i>
          </button>
        </div>
      </div>
      <Transition name="drop">
        <nav v-if="mobileMenu" class="md:hidden border-t border-[#EDE4F5] bg-white px-6 py-4 flex flex-col gap-4 text-[15px]">
          <button v-for="l in ['services', 'approach', 'calm', 'faq', 'book']" :key="l" class="text-left capitalize text-[#6B5E78]" @click="scrollToId(l)">{{ l === 'calm' ? 'Calm space' : l }}</button>
        </nav>
      </Transition>
    </header>

    <!-- ============ HERO ============ -->
    <section class="relative overflow-hidden">
      <div class="blob absolute -top-32 -right-32 h-96 w-96 rounded-full opacity-40 blur-3xl" style="background: radial-gradient(circle, #D9B8F0 0%, transparent 70%)"></div>
      <div class="blob-slow absolute bottom-0 -left-24 h-72 w-72 rounded-full opacity-30 blur-3xl" style="background: radial-gradient(circle, #F9C9E2 0%, transparent 70%)"></div>

      <div class="max-w-7xl mx-auto px-6 lg:px-10 pt-16 pb-20 lg:pt-24 lg:pb-28 grid lg:grid-cols-2 gap-14 items-center relative">
        <div data-aos="fade-right">
          <span class="inline-flex items-center gap-2 bg-[#F3EAFA] text-[#5B1A8F] text-[13px] font-semibold px-3.5 py-1.5 rounded-full mb-5">
            <span class="relative flex h-2 w-2"><span class="absolute inline-flex h-full w-full rounded-full bg-[#0E9F5A] opacity-60 animate-ping"></span><span class="relative inline-flex h-2 w-2 rounded-full bg-[#0E9F5A]"></span></span>
            Accepting new clients
          </span>
          <h1 class="if-heading text-[38px] sm:text-[48px] lg:text-[56px] font-bold leading-[1.05] mb-5">
            A calmer mind starts with a conversation.
          </h1>
          <p class="text-[16px] lg:text-[17px] text-[#6B5E78] leading-relaxed mb-8 max-w-md">
            I'm Lucy Kim, a psychotherapist and counsellor offering warm, evidence-based therapy for anxiety, trauma, relationships and burnout, online and in person.
          </p>
          <div class="flex flex-wrap items-center gap-4 mb-8">
            <button @click="scrollToId('book')" class="btn-shine bg-[#5B1A8F] hover:bg-[#46107A] transition-colors text-white text-[15px] font-semibold px-6 py-3 rounded-full flex items-center gap-2">
              <i class="mdi mdi-calendar-heart text-[18px]"></i>
              Book a free intro call
            </button>
            <button @click="scrollToId('calm')" class="border border-[#DDD0EB] hover:border-[#5B1A8F] hover:text-[#5B1A8F] transition-colors text-[15px] font-semibold px-6 py-3 rounded-full">
              Try a breathing exercise
            </button>
          </div>
          <div class="flex flex-wrap gap-x-6 gap-y-2 text-[13px] text-[#6B5E78]">
            <span class="flex items-center gap-1.5"><i class="mdi mdi-shield-check text-[#5B1A8F]"></i>Licensed &amp; confidential</span>
            <span class="flex items-center gap-1.5"><i class="mdi mdi-video-outline text-[#5B1A8F]"></i>Online &amp; in person</span>
            <span class="flex items-center gap-1.5"><i class="mdi mdi-clock-outline text-[#5B1A8F]"></i>Evenings available</span>
          </div>
        </div>

        <div data-aos="fade-left" data-aos-delay="150" class="relative h-[360px] lg:h-[440px]">
          <div class="absolute inset-0 rounded-[32px] overflow-hidden bg-[#1E0A33] tilt">
            <img :src="profilePic" alt="Lucy Kim" class="h-full w-full object-cover object-top" />
            <div class="absolute inset-0 bg-gradient-to-t from-[#1E0A33]/70 via-transparent to-transparent"></div>
          </div>
         
        </div>
      </div>
    </section>

    <!-- ============ STATS STRIP ============ -->
    <section ref="statsEl" class="border-y border-[#EDE4F5] bg-[#FAF6FD]">
      <div class="max-w-7xl mx-auto px-6 lg:px-10 py-10 grid grid-cols-2 lg:grid-cols-4 gap-8">
        <div v-for="(s, i) in stats" :key="s.label" class="text-center lg:text-left">
          <p class="if-heading text-[28px] lg:text-[32px] font-bold text-[#5B1A8F] tabular-nums">{{ statValues[i].toLocaleString() }}{{ s.suffix }}</p>
          <p class="text-[13px] text-[#6B5E78]">{{ s.label }}</p>
        </div>
      </div>
    </section>

    <!-- ============ MARQUEE ============ -->
    <section class="py-6 overflow-hidden border-b border-[#EDE4F5]" aria-label="Areas of support">
      <div class="marquee flex gap-3 w-max">
        <span v-for="(s, i) in [...specialties, ...specialties]" :key="i" class="whitespace-nowrap text-[13.5px] text-[#6B5E78] border border-[#EDE4F5] rounded-full px-4 py-2 bg-white">{{ s }}</span>
      </div>
    </section>

    <!-- ============ SERVICES ============ -->
    <section id="services" class="max-w-7xl mx-auto px-6 lg:px-10 py-20 lg:py-28">
      <div class="text-center max-w-xl mx-auto mb-14" data-aos="fade-up">
        <h2 class="if-heading text-[30px] lg:text-[36px] font-bold mb-3">How I can help</h2>
        <p class="text-[15px] text-[#6B5E78] leading-relaxed">Compassionate, evidence-based support shaped around your life, your goals and your pace.</p>
      </div>
      <div class="grid md:grid-cols-2 gap-6">
        <div v-for="(service, i) in services" :key="service.title" data-aos="fade-up" :data-aos-delay="i * 120"
          class="group rounded-2xl border border-[#EDE4F5] hover:border-[#5B1A8F] hover:shadow-lg transition-all p-6 lg:p-7">
          <div class="h-11 w-11 rounded-xl bg-[#F3EAFA] group-hover:bg-[#5B1A8F] transition-colors flex items-center justify-center mb-4 shrink-0">
            <i class="mdi text-[#5B1A8F] group-hover:text-white transition-colors text-[20px]" :class="service.icon"></i>
          </div>
          <p class="text-[15px] font-semibold mb-2">{{ service.title }}</p>
          <p class="text-[13.5px] text-[#6B5E78] leading-relaxed">{{ service.copy }}</p>
        </div>
      </div>
    </section>

    <!-- ============ ABOUT ============ -->
    <section class="bg-[#FAF6FD] py-20 lg:py-28">
      <div class="max-w-7xl mx-auto px-6 lg:px-10 grid lg:grid-cols-2 gap-14 items-center">
        <div data-aos="fade-right">
          <h2 class="if-heading text-[30px] lg:text-[36px] font-bold mb-4">Meet Lucy</h2>
          <p class="text-[15px] text-[#6B5E78] leading-relaxed mb-4">
            Replace with Lucy's story: why she became a psychotherapist, her therapeutic style and what clients can expect in the room.
          </p>
          <p class="text-[15px] text-[#6B5E78] leading-relaxed mb-6">
            My approach is collaborative and practical. You will leave each session with a clearer understanding of yourself and tools you can use straight away.
          </p>
          <div class="flex flex-wrap gap-2">
            <span v-for="s in specialties.slice(0, 6)" :key="s" class="text-[12.5px] bg-white border border-[#EDE4F5] rounded-full px-3 py-1.5 text-[#4A3D58]">{{ s }}</span>
          </div>
        </div>
        <div class="grid gap-4">
          <div v-for="(c, i) in credentials" :key="c.title" data-aos="fade-left" :data-aos-delay="i * 120"
            class="bg-white rounded-2xl border border-[#EDE4F5] p-5 flex items-start gap-4 hover:shadow-lg hover:-translate-y-0.5 transition-all">
            <div class="h-11 w-11 rounded-xl bg-[#F3EAFA] flex items-center justify-center shrink-0"><i class="mdi text-[#5B1A8F] text-[20px]" :class="c.icon"></i></div>
            <div><p class="text-[15px] font-semibold mb-1">{{ c.title }}</p><p class="text-[13px] text-[#9A8FA8]">{{ c.copy }}</p></div>
          </div>
        </div>
      </div>
    </section>

    <!-- ============ APPROACH (timeline) ============ -->
    <section id="approach" class="max-w-7xl mx-auto px-6 lg:px-10 py-20 lg:py-28">
      <div class="text-center max-w-xl mx-auto mb-14" data-aos="fade-up">
        <h2 class="if-heading text-[30px] lg:text-[36px] font-bold mb-3">What to expect</h2>
        <p class="text-[15px] text-[#6B5E78]">Four simple steps from first hello to lasting change.</p>
      </div>
      <div class="relative grid md:grid-cols-4 gap-6">
        <div class="hidden md:block absolute top-[22px] left-[12%] right-[12%] h-px bg-gradient-to-r from-transparent via-[#D9B8F0] to-transparent"></div>
        <div v-for="(step, i) in approach" :key="step.title" data-aos="fade-up" :data-aos-delay="i * 130" class="relative text-center">
          <div class="relative mx-auto h-11 w-11 rounded-full bg-[#5B1A8F] text-white flex items-center justify-center mb-4 ring-8 ring-white">
            <i class="mdi text-[20px]" :class="step.icon"></i>
          </div>
          <p class="text-[15px] font-semibold mb-2">{{ step.title }}</p>
          <p class="text-[13.5px] text-[#6B5E78] leading-relaxed">{{ step.copy }}</p>
        </div>
      </div>
    </section>

    <!-- ============ CALM SPACE (mood + breathing) ============ -->
    <section id="calm" class="bg-[#1E0A33] py-20 lg:py-28 relative overflow-hidden">
      <div class="absolute inset-0 opacity-90" style="background: radial-gradient(circle at 80% 20%, #5B1A8F 0%, #1E0A33 60%)"></div>
      <div class="max-w-7xl mx-auto px-6 lg:px-10 relative z-10">
        <div class="text-center max-w-xl mx-auto mb-14" data-aos="fade-up">
          <h2 class="if-heading text-white text-[30px] lg:text-[36px] font-bold mb-3">Take a moment for yourself</h2>
          <p class="text-[15px] text-[#CDBFDD]">A quick check-in and a guided breathing exercise. No sign-up needed.</p>
        </div>
        <div class="grid lg:grid-cols-2 gap-8">
          <!-- mood -->
          <div data-aos="fade-up" class="rounded-3xl bg-white/5 border border-white/10 backdrop-blur p-7">
            <p class="if-heading text-white text-[18px] font-bold mb-1">How are you feeling today?</p>
            <p class="text-[13.5px] text-[#CDBFDD] mb-6">Tap the one that fits best.</p>
            <div class="grid grid-cols-4 gap-3 mb-6">
              <button v-for="(m, i) in moods" :key="m.label" @click="mood = i"
                class="rounded-2xl py-4 flex flex-col items-center gap-2 text-[12px] transition-all border"
                :class="mood === i ? 'bg-[#D4147E] border-[#D4147E] text-white scale-105' : 'bg-white/5 border-white/10 text-[#CDBFDD] hover:bg-white/10'">
                <i class="mdi text-[28px]" :class="m.icon"></i>{{ m.label }}
              </button>
            </div>
            <Transition name="fade" mode="out-in">
              <div v-if="mood !== null" :key="mood" class="rounded-2xl bg-white/10 p-5">
                <p class="text-white text-[14px] leading-relaxed mb-4">{{ moods[mood].msg }}</p>
                <button @click="openWhatsApp(`Hi Lucy, I checked in as feeling ${moods[mood].label.toLowerCase()} and would like to talk.`)" class="bg-white text-[#5B1A8F] text-[13.5px] font-semibold px-5 py-2.5 rounded-full hover:bg-white/90 transition-colors">Talk to Lucy</button>
              </div>
              <p v-else class="text-[13px] text-[#CDBFDD]/70">This check-in is private and is not stored.</p>
            </Transition>
          </div>

          <!-- breathing -->
          <div data-aos="fade-up" data-aos-delay="120" class="rounded-3xl bg-white/5 border border-white/10 backdrop-blur p-7 flex flex-col items-center text-center">
            <p class="if-heading text-white text-[18px] font-bold mb-1">Guided breathing</p>
            <p class="text-[13.5px] text-[#CDBFDD] mb-6">Follow the circle: in for 4, hold for 4, out for 6.</p>
            <div class="relative h-48 w-48 flex items-center justify-center mb-6">
              <div class="absolute inset-0 rounded-full bg-[#D4147E]/25 ease-in-out transition-transform" :style="circleStyle"></div>
              <div class="absolute inset-6 rounded-full bg-[#D4147E]/45 ease-in-out transition-transform" :style="circleStyle"></div>
              <div class="relative z-10 text-white">
                <p class="if-heading text-[18px] font-bold">{{ breathing ? phases[phaseIdx].name : 'Ready?' }}</p>
                <p v-if="breathing" class="text-[28px] font-bold tabular-nums">{{ countdown }}</p>
              </div>
            </div>
            <button @click="toggleBreathing" class="bg-[#D4147E] hover:bg-[#B80F6C] transition-colors text-white text-[14px] font-semibold px-6 py-3 rounded-full">
              {{ breathing ? 'Stop' : 'Start breathing' }}
            </button>
          </div>
        </div>
      </div>
    </section>

    <!-- ============ TESTIMONIALS (carousel) ============ -->
    <section class="max-w-7xl mx-auto px-6 lg:px-10 py-20 lg:py-28">
      <div class="text-center max-w-xl mx-auto mb-12" data-aos="fade-up">
        <h2 class="if-heading text-[30px] lg:text-[36px] font-bold mb-3">Kind words from clients</h2>
        <p class="text-[15px] text-[#6B5E78]">Shared with permission and anonymised for privacy.</p>
      </div>
      <div data-aos="fade-up" class="max-w-2xl mx-auto rounded-3xl border border-[#EDE4F5] p-8 text-center">
        <i class="fa-solid fa-quote-left text-[#D9B8F0] text-2xl mb-4"></i>
        <Transition name="fade" mode="out-in">
          <div :key="activeT">
            <p class="text-[16px] text-[#4A3D58] leading-relaxed mb-6">{{ testimonials[activeT].quote }}</p>
            <div class="flex items-center justify-center gap-3">
              <div class="h-10 w-10 rounded-full bg-[#5B1A8F] flex items-center justify-center text-white text-[12px] font-semibold">{{ testimonials[activeT].name.slice(0, 1) }}</div>
              <div class="text-left"><p class="text-[13px] font-semibold">{{ testimonials[activeT].name }}</p><p class="text-[11.5px] text-[#9A8FA8]">{{ testimonials[activeT].role }}</p></div>
            </div>
          </div>
        </Transition>
        <div class="flex justify-center gap-2 mt-6">
          <button v-for="(t, i) in testimonials" :key="i" @click="pickT(i)" :aria-label="`Testimonial ${i + 1}`"
            class="h-2 rounded-full transition-all" :class="activeT === i ? 'w-6 bg-[#5B1A8F]' : 'w-2 bg-[#DDD0EB]'"></button>
        </div>
      </div>
    </section>

    <!-- ============ RESOURCES ============ -->
    <section class="bg-[#FAF6FD] py-20 lg:py-28">
      <div class="max-w-7xl mx-auto px-6 lg:px-10">
        <div class="flex items-end justify-between mb-10" data-aos="fade-up">
          <div>
            <h2 class="if-heading text-[30px] lg:text-[36px] font-bold mb-2">Helpful reads</h2>
            <p class="text-[15px] text-[#6B5E78]">Practical guidance for everyday mental wellbeing.</p>
          </div>
          <a href="#" class="hidden sm:flex items-center gap-1 text-[14px] font-semibold text-[#5B1A8F] hover:underline">All articles<i class="mdi mdi-chevron-right"></i></a>
        </div>
        <div class="grid sm:grid-cols-2 lg:grid-cols-3 gap-6">
          <a v-for="(r, i) in resources" :key="r.title" href="#" data-aos="fade-up" :data-aos-delay="i * 100"
            class="bg-white rounded-2xl border border-[#EDE4F5] overflow-hidden hover:shadow-lg hover:-translate-y-1 transition-all group block">
            <div class="aspect-[16/10] bg-[#F3EAFA] flex items-center justify-center overflow-hidden">
              <i class="mdi mdi-book-open-page-variant-outline text-[#5B1A8F] text-[44px] group-hover:scale-125 transition-transform duration-500"></i>
            </div>
            <div class="p-5">
              <span class="text-[11.5px] font-semibold text-[#D4147E] bg-[#FCE7F3] rounded-full px-2.5 py-1">{{ r.tag }}</span>
              <p class="text-[15px] font-semibold mt-3 mb-1">{{ r.title }}</p>
              <p class="text-[12.5px] text-[#9A8FA8]">{{ r.time }}</p>
            </div>
          </a>
        </div>
      </div>
    </section>

    <!-- ============ BOOKING ============ -->
    <section id="book" class="max-w-7xl mx-auto px-6 lg:px-10 py-20 lg:py-28">
      <div class="text-center max-w-xl mx-auto mb-12" data-aos="fade-up">
        <h2 class="if-heading text-[30px] lg:text-[36px] font-bold mb-3">Book a session</h2>
        <p class="text-[15px] text-[#6B5E78]">Choose a format, date and time, then confirm on WhatsApp.</p>
      </div>

      <div data-aos="fade-up" class="max-w-md mx-auto rounded-3xl border border-[#EDE4F5] p-6">
        <div class="flex bg-[#F6F0FB] rounded-full p-1 mb-5">
          <button v-for="t in sessionTypes" :key="t" @click="sessionType = t" class="flex-1 py-2 rounded-full text-[13px] font-semibold transition-all"
            :class="sessionType === t ? 'bg-white text-[#5B1A8F] shadow-sm' : 'text-[#6B5E78]'">{{ t }}</button>
        </div>

        <div class="flex items-center justify-between mb-5">
          <button @click="changeMonth(-1)" :disabled="!canGoBack" class="h-9 w-9 rounded-full hover:bg-[#F6F0FB] disabled:opacity-30" aria-label="Previous month"><i class="mdi mdi-chevron-left"></i></button>
          <p class="if-heading font-bold">{{ monthLabel }}</p>
          <button @click="changeMonth(1)" class="h-9 w-9 rounded-full hover:bg-[#F6F0FB]" aria-label="Next month"><i class="mdi mdi-chevron-right"></i></button>
        </div>

        <div class="grid grid-cols-7 gap-1 text-center text-[11.5px] text-[#9A8FA8] mb-2">
          <span v-for="(d, i) in ['S', 'M', 'T', 'W', 'T', 'F', 'S']" :key="i">{{ d }}</span>
        </div>
        <div class="grid grid-cols-7 gap-1">
          <template v-for="(c, i) in calendarCells" :key="i">
            <div v-if="!c"></div>
            <button v-else :disabled="!c.available" @click="pickDate(c)" class="aspect-square rounded-xl text-[13px] font-medium transition-all"
              :class="selectedDate === c.key ? 'bg-[#5B1A8F] text-white scale-105'
                : c.available ? 'bg-[#F3EAFA] text-[#5B1A8F] hover:bg-[#5B1A8F] hover:text-white'
                : c.booked ? 'bg-[#FDECEC] text-[#D64545] line-through cursor-not-allowed'
                : 'text-[#CFC6D8] cursor-not-allowed'">{{ c.d }}</button>
          </template>
        </div>

        <Transition name="drop">
          <div v-if="selectedDate" class="mt-6">
            <p class="text-[13px] font-semibold mb-3">Pick a time on {{ selectedDate }}</p>
            <div class="grid grid-cols-3 gap-2 mb-5">
              <button v-for="s in slots" :key="s" @click="selectedSlot = s" class="py-2 rounded-xl text-[13px] font-medium border transition-colors"
                :class="selectedSlot === s ? 'bg-[#5B1A8F] border-[#5B1A8F] text-white' : 'border-[#EDE4F5] text-[#6B5E78] hover:border-[#5B1A8F]'">{{ s }}</button>
            </div>
            <button :disabled="!selectedSlot" @click="confirmBooking" class="w-full bg-[#5B1A8F] hover:bg-[#46107A] disabled:opacity-40 disabled:cursor-not-allowed transition-colors text-white text-[14px] font-semibold py-3 rounded-full">
              Confirm on WhatsApp
            </button>
          </div>
        </Transition>

        <div class="flex items-center justify-center gap-5 mt-5 text-[12px] text-[#6B5E78]">
          <span class="flex items-center gap-1.5"><i class="h-2.5 w-2.5 rounded-full bg-[#5B1A8F]"></i>Available</span>
          <span class="flex items-center gap-1.5"><i class="h-2.5 w-2.5 rounded-full bg-[#D64545]"></i>Booked</span>
        </div>
      </div>
    </section>

    <!-- ============ PRICING ============ -->
    <section class="bg-[#FAF6FD] py-20 lg:py-28">
      <div class="max-w-7xl mx-auto px-6 lg:px-10">
        <div class="text-center max-w-xl mx-auto mb-14" data-aos="fade-up">
          <h2 class="if-heading text-[30px] lg:text-[36px] font-bold mb-3">Ways to work together</h2>
          <p class="text-[15px] text-[#6B5E78] leading-relaxed">Clear options with no pressure. Start with a free intro call.</p>
        </div>
        <div class="grid md:grid-cols-3 gap-6">
          <div v-for="(plan, i) in plans" :key="plan.name" data-aos="fade-up" :data-aos-delay="i * 120"
            class="rounded-3xl p-7 flex flex-col hover:-translate-y-1 transition-transform" :class="plan.tag ? 'bg-[#5B1A8F]' : 'bg-white border border-[#EDE4F5]'">
            <span v-if="plan.tag" class="self-start text-[11px] font-semibold text-[#1E0A33] bg-white rounded-full px-3 py-1 mb-4">{{ plan.tag }}</span>
            <p class="if-heading text-[18px] font-bold mb-1" :class="plan.tag ? 'text-white' : 'text-[#1E0F2E]'">{{ plan.name }}</p>
            <div class="flex items-baseline gap-1 mb-6">
              <span class="text-[34px] font-bold" :class="plan.tag ? 'text-white' : 'text-[#5B1A8F]'">{{ plan.price }}</span>
              <span class="text-[13px]" :class="plan.tag ? 'text-white/80' : 'text-[#9A8FA8]'">{{ plan.period }}</span>
            </div>
            <ul class="flex flex-col gap-3 mb-8 flex-1">
              <li v-for="f in plan.features" :key="f" class="flex items-start gap-2 text-[13.5px]" :class="plan.tag ? 'text-white/90' : 'text-[#6B5E78]'">
                <i class="mdi mdi-check-circle text-[16px] mt-0.5 shrink-0" :class="plan.tag ? 'text-white' : 'text-[#0E9F5A]'"></i>{{ f }}
              </li>
            </ul>
            <button @click="openWhatsApp(`Hi Lucy, I'm interested in the ${plan.name} option. Could you share more details?`)"
              class="w-full py-3 rounded-full text-[14px] font-semibold transition-colors" :class="plan.tag ? 'bg-white text-[#5B1A8F] hover:bg-white/90' : 'bg-[#F3EAFA] text-[#5B1A8F] hover:bg-[#E6D6F4]'">{{ plan.cta }}</button>
          </div>
        </div>
      </div>
    </section>

    <!-- ============ FAQ ============ -->
    <section id="faq" class="max-w-3xl mx-auto px-6 lg:px-10 py-20 lg:py-28">
      <div class="text-center mb-12" data-aos="fade-up">
        <h2 class="if-heading text-[30px] lg:text-[36px] font-bold mb-3">Common questions</h2>
        <p class="text-[15px] text-[#6B5E78]">Everything you might want to know before reaching out.</p>
      </div>
      <div class="flex flex-col gap-3">
        <div v-for="(f, i) in faqs" :key="f.q" data-aos="fade-up" :data-aos-delay="i * 60" class="rounded-2xl border transition-colors" :class="openFaq === i ? 'border-[#5B1A8F]' : 'border-[#EDE4F5]'">
          <button class="w-full flex items-center justify-between gap-4 text-left px-5 py-4" :aria-expanded="openFaq === i" @click="openFaq = openFaq === i ? -1 : i">
            <span class="text-[15px] font-semibold">{{ f.q }}</span>
            <i class="mdi mdi-chevron-down text-[20px] text-[#5B1A8F] transition-transform duration-300" :class="openFaq === i ? 'rotate-180' : ''"></i>
          </button>
          <div class="grid transition-all duration-300" :style="{ gridTemplateRows: openFaq === i ? '1fr' : '0fr' }">
            <div class="overflow-hidden"><p class="px-5 pb-5 text-[14px] text-[#6B5E78] leading-relaxed">{{ f.a }}</p></div>
          </div>
        </div>
      </div>
    </section>

    <!-- ============ CTA BANNER ============ -->
    <section class="max-w-7xl mx-auto px-6 lg:px-10 pb-20 lg:pb-28">
      <div data-aos="zoom-in" class="relative overflow-hidden rounded-[32px] bg-[#1E0A33] px-8 py-14 lg:py-16 text-center">
        <div class="absolute inset-0 opacity-90" style="background: radial-gradient(circle at 20% 20%, #5B1A8F 0%, #1E0A33 65%)"></div>
        <div class="relative z-10 max-w-lg mx-auto">
          <h2 class="if-heading text-white text-[28px] lg:text-[34px] font-bold mb-4">Ready to take the first step?</h2>
          <p class="text-[#CDBFDD] text-[15px] mb-8">Reaching out is often the hardest part. I'm here when you're ready.</p>
          <div class="flex flex-wrap justify-center gap-4">
            <button @click="openWhatsApp('Hi Lucy, I would like to book a free intro call.')" class="btn-shine bg-[#D4147E] hover:bg-[#B80F6C] transition-colors text-white text-[15px] font-semibold px-6 py-3 rounded-full">Book intro call</button>
            <button @click="openWhatsApp('Hi Lucy, I have a question before booking.')" class="border border-white/20 hover:border-white/40 transition-colors text-white text-[15px] font-semibold px-6 py-3 rounded-full">Ask a question</button>
          </div>
        </div>
      </div>
    </section>

    <!-- ============ FOOTER ============ -->
    <footer class="border-t border-[#EDE4F5]">
      <div class="max-w-7xl mx-auto px-6 lg:px-10 py-14 grid sm:grid-cols-2 lg:grid-cols-5 gap-10">
        <div class="lg:col-span-2">
          <div class="flex items-center gap-2 mb-4">
            <img :src="logo" alt="Lucy Kim, Psychotherapist & Counsellor" class="h-14 w-auto object-contain" />
          </div>
          <p class="text-[13.5px] text-[#6B5E78] leading-relaxed mb-5 max-w-xs">Psychotherapist &amp; counsellor offering compassionate, evidence-based therapy online and in person.</p>
          <div class="flex items-center gap-3">
            <a href="#" aria-label="LinkedIn" class="h-9 w-9 rounded-full bg-[#F6F0FB] flex items-center justify-center text-[#6B5E78] hover:text-white hover:bg-[#5B1A8F] transition-colors"><i class="fa-brands fa-linkedin text-xs"></i></a>
            <a href="#" aria-label="Instagram" class="h-9 w-9 rounded-full bg-[#F6F0FB] flex items-center justify-center text-[#6B5E78] hover:text-white hover:bg-[#5B1A8F] transition-colors"><i class="fa-brands fa-instagram text-xs"></i></a>
            <a href="#" aria-label="X" class="h-9 w-9 rounded-full bg-[#F6F0FB] flex items-center justify-center text-[#6B5E78] hover:text-white hover:bg-[#5B1A8F] transition-colors"><i class="fa-brands fa-x-twitter text-xs"></i></a>
            <a href="mailto:hello@example.com" aria-label="Email" class="h-9 w-9 rounded-full bg-[#F6F0FB] flex items-center justify-center text-[#6B5E78] hover:text-white hover:bg-[#5B1A8F] transition-colors"><i class="fa-solid fa-envelope text-xs"></i></a>
          </div>
        </div>
        <div v-for="(links, section) in footerLinks" :key="section">
          <p class="text-[13px] font-semibold mb-4 text-[#1E0F2E]">{{ section }}</p>
          <ul class="flex flex-col gap-3">
            <li v-for="link in links" :key="link"><a href="#" class="text-[13.5px] text-[#6B5E78] hover:text-[#5B1A8F]">{{ link }}</a></li>
          </ul>
        </div>
      </div>
      <div class="border-t border-[#EDE4F5]">
        <div class="max-w-7xl mx-auto px-6 lg:px-10 py-5 flex flex-col sm:flex-row items-center justify-between gap-2">
          <p class="text-[12.5px] text-[#9A8FA8]">© 2026 Lucy Kim. All rights reserved.</p>
          <p class="text-[12.5px] text-[#9A8FA8] text-center sm:text-right">Not an emergency service. In a crisis, contact your local emergency number or crisis line.</p>
        </div>
      </div>
    </footer>

    <!-- floating actions -->
    <Transition name="fade">
      <button v-if="scrollY > 600" @click="toTop" aria-label="Back to top" class="fixed bottom-6 left-6 z-40 h-11 w-11 rounded-full bg-white border border-[#EDE4F5] shadow-lg hover:border-[#5B1A8F] text-[#5B1A8F]"><i class="mdi mdi-arrow-up"></i></button>
    </Transition>
    <button @click="openWhatsApp('Hi Lucy')" aria-label="Chat on WhatsApp" class="fixed bottom-6 right-6 z-40 h-14 w-14 rounded-full bg-[#0E9F5A] hover:bg-[#0B8049] text-white shadow-xl flex items-center justify-center pulse-ring">
      <i class="fa-brands fa-whatsapp text-[26px]"></i>
    </button>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Manrope:wght@600;700;800&family=Inter:wght@400;500;600&display=swap');

.if-shell { font-family: 'Inter', ui-sans-serif, system-ui, sans-serif; }
.if-heading { font-family: 'Manrope', ui-sans-serif, system-ui, sans-serif; letter-spacing: -0.01em; }

/* ambient motion */
.blob { animation: drift 14s ease-in-out infinite alternate; }
.blob-slow { animation: drift 20s ease-in-out infinite alternate-reverse; }
@keyframes drift { to { transform: translate(40px, 30px) scale(1.12); } }
.float { animation: float 5s ease-in-out infinite; }
.float-delay { animation: float 6s ease-in-out 1s infinite; }
@keyframes float { 50% { transform: translateY(-10px); } }
.tilt { transition: transform 0.6s ease; }
.tilt:hover { transform: perspective(900px) rotateY(-3deg) rotateX(2deg); }

/* marquee */
.marquee { animation: scroll 35s linear infinite; }
.marquee:hover { animation-play-state: paused; }
@keyframes scroll { to { transform: translateX(-50%); } }

/* shine button */
.btn-shine { position: relative; overflow: hidden; }
.btn-shine::after {
  content: ''; position: absolute; top: 0; left: -75%; width: 50%; height: 100%;
  background: linear-gradient(120deg, transparent, rgba(255, 255, 255, 0.35), transparent);
  transform: skewX(-20deg); transition: left 0.7s ease;
}
.btn-shine:hover::after { left: 130%; }

/* whatsapp pulse */
.pulse-ring { animation: pulse 2.6s infinite; }
@keyframes pulse {
  0% { box-shadow: 0 0 0 0 rgba(15, 157, 88, 0.5); }
  70% { box-shadow: 0 0 0 16px rgba(15, 157, 88, 0); }
  100% { box-shadow: 0 0 0 0 rgba(15, 157, 88, 0); }
}

/* transitions */
.fade-enter-active, .fade-leave-active { transition: opacity 0.3s ease, transform 0.3s ease; }
.fade-enter-from, .fade-leave-to { opacity: 0; transform: translateY(8px); }
.drop-enter-active, .drop-leave-active { transition: opacity 0.3s ease, transform 0.3s ease; }
.drop-enter-from, .drop-leave-to { opacity: 0; transform: translateY(-8px); }

@media (prefers-reduced-motion: reduce) {
  .blob, .blob-slow, .float, .float-delay, .marquee, .pulse-ring { animation: none; }
  .tilt, .btn-shine::after { transition: none; }
}
</style>