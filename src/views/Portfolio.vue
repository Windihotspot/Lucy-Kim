<script setup>
import { ref, reactive, onMounted, computed } from 'vue'
import AOS from 'aos'
import 'aos/dist/aos.css'

import profilePic from '@/assets/profile.jpeg' // add your photo here

// thumbnail helper (live screenshot of each site)
const shot = (url) => `https://image.thum.io/get/width/700/crop/450/${url}`

onMounted(() => {
  AOS.init({ duration: 800, once: true, easing: 'ease-out-cubic', offset: 80 })
})

const stats = [
  { label: 'Active Users', value: '50k+' },
  { label: 'Transactions Processed', value: '₦83.5M' },
  { label: 'Platforms Built', value: '6+' },
  { label: 'Uptime', value: '99.9%' }
]

const services = [
  {
    icon: 'mdi-finance',
    title: 'Fintech Platforms',
    copy: 'Build lending, payment, and loan management systems. Real-time analytics, secure transactions, and scalable infrastructure.'
  },
  {
    icon: 'mdi-database',
    title: 'Data & Analytics',
    copy: 'Real-time dashboards, charts, and business intelligence. Transform raw data into actionable insights for decision-making.'
  },
  {
    icon: 'mdi-shield-check',
    title: 'Security & Compliance',
    copy: 'KYC systems, user verification, and regulatory compliance. Enterprise-grade security for sensitive financial data.'
  },
  {
    icon: 'mdi-mobile-screen-multiple',
    title: 'Web & Mobile Apps',
    copy: 'Responsive web applications and cross-platform mobile solutions using modern frameworks and best practices.'
  }
]

const featuredProjects = reactive([
  {
    id: 1,
    title: 'Credmate',
    description:
      'A digital lending platform built to simplify loan applications and automate credit workflows.',
    impact: '50k+ users',
    link: 'https://www.getcredmate.co/',
    image: shot('https://www.getcredmate.co/')
  },
  {
    id: 2,
    title: 'Jupita',
    description:
      'A loan and biometric management dashboard for verification, loan processing and operations.',
    impact: '₦83.5M processed',
    link: 'https://app.getjupita.com/',
    image: shot('https://app.getjupita.com/')
  },
  {
    id: 3,
    title: 'Quidly',
    description:
      'A merchant management platform for onboarding, verification and real-time visibility.',
    impact: 'Real-time verification',
    link: 'https://www.quidly.ng/',
    image: shot('https://www.quidly.ng/')
  },
  {
    id: 4,
    title: 'Sell Desk',
    description: 'An invoice and payment management system to create, track and collect payments.',
    impact: '10k+ invoices/month',
    link: 'https://www.quidly.ng/selldesk',
    image: shot('https://www.quidly.ng/selldesk')
  },
  {
    id: 5,
    title: 'Wealth Oceans(Sales Closer Intelligence Layer)',
    description: 'AI built to qualify leads, handle objections and close across every channel.',
    impact: 'Replace with impact',
    link: 'https://www.wealthoceanstech.com/',
    image: shot('https://www.wealthoceanstech.com/')
  }
])
const technologies = [
  { name: 'Vue.js', level: 'Expert' },
  { name: 'React', level: 'Advanced' },
  { name: 'Node.js', level: 'Expert' },
  { name: 'Laravel', level: 'Advanced' },
  { name: 'PostgreSQL', level: 'Advanced' },
  { name: 'MongoDB', level: 'Advanced' },
  { name: 'AWS', level: 'Intermediate' },
  { name: 'TypeScript', level: 'Advanced' },
  { name: 'REST APIs', level: 'Expert' },
  { name: 'Real-time Systems', level: 'Advanced' }
]

const testimonials = [
  {
    quote:
      'Ifiok built our entire lending platform from concept to production. The system handles 50k+ users seamlessly, with real-time analytics and zero downtime. Exceptional attention to detail.',
    name: 'Williams Adeyemi',
    role: 'CEO, Credmate',
    company: 'Fintech Startup'
  },
  {
    quote:
      'The invoice and payment system he developed for us processes thousands of transactions monthly. The architecture is robust, scalable, and the code quality is outstanding.',
    name: 'Taiwo Efunogbon',
    role: 'Director, Quidly',
    company: 'Payment Platform'
  },
  {
    quote:
      'From the admin dashboard to user KYC verification — everything works flawlessly. Ifiok understands fintech complexity and delivers enterprise-grade solutions. Highly recommended.',
    name: 'Esther Umoh',
    role: 'Operations Lead, TeinnovateLabs',
    company: 'Fintech Innovation'
  }
]

const plans = [
  {
    name: 'Consultation',
    price: 'Custom',
    period: 'per project',
    tag: null,
    features: [
      'Architecture planning',
      'Technical strategy',
      '2-week delivery',
      'Post-launch support'
    ],
    cta: 'Schedule call'
  },
  {
    name: 'Development',
    price: 'Custom',
    period: 'per scope',
    tag: 'Most popular',
    features: [
      'Full-stack solutions',
      'Agile delivery',
      'Weekly syncs',
      'Unlimited revisions',
      'Code ownership'
    ],
    cta: 'Start project'
  },
  {
    name: 'Maintenance',
    price: 'Custom',
    period: 'per month',
    tag: null,
    features: [
      '24/7 monitoring',
      'Performance optimization',
      'Security updates',
      'Priority support'
    ],
    cta: 'Get managed'
  }
]

const footerLinks = {
  Work: ['Featured projects', 'Technologies', 'Services', 'Testimonials'],
  Connect: ['Email', 'LinkedIn', 'GitHub', 'Twitter'],
  Resources: ['Blog', 'Resume', 'Case studies', 'Pricing']
}

const email = ref('')
const scrollY = ref(0)

onMounted(() => {
  window.addEventListener('scroll', () => {
    scrollY.value = window.scrollY
  })
})
const whatsappNumber = '2349132378328'

const contactOnWhatsApp = (planName) => {
  const message = `Hi Ifiok, I'm interested in your ${planName} plan. I'd like to discuss my project and get more details.`

  const url = `https://wa.me/${whatsappNumber}?text=${encodeURIComponent(message)}`

  window.open(url, '_blank')
}
const openWhatsApp = (message = '') => {
  const url = message
    ? `https://wa.me/${whatsappNumber}?text=${encodeURIComponent(message)}`
    : `https://wa.me/${whatsappNumber}`

  window.open(url, '_blank')
}
// ---- Availability calendar ----
const workDays = [1, 2, 3, 4, 5] // Mon–Fri available
const bookedDates = ['2026-11-12', '2026-11-18'] // YYYY-MM-DD, edit these
const now = new Date()
const todayStart = new Date(now.getFullYear(), now.getMonth(), now.getDate())
const viewDate = ref(new Date(now.getFullYear(), now.getMonth(), 1))

const monthLabel = computed(() =>
  viewDate.value.toLocaleDateString('en-GB', { month: 'long', year: 'numeric' })
)
const canGoBack = computed(() => viewDate.value > new Date(now.getFullYear(), now.getMonth(), 1))
const changeMonth = (n) => {
  const d = viewDate.value
  viewDate.value = new Date(d.getFullYear(), d.getMonth() + n, 1)
}
const calendarCells = computed(() => {
  const y = viewDate.value.getFullYear()
  const m = viewDate.value.getMonth()
  const cells = Array(new Date(y, m, 1).getDay()).fill(null)
  for (let d = 1; d <= new Date(y, m + 1, 0).getDate(); d++) {
    const date = new Date(y, m, d)
    const key = `${y}-${String(m + 1).padStart(2, '0')}-${String(d).padStart(2, '0')}`
    const booked = bookedDates.includes(key)
    cells.push({
      d,
      key,
      booked,
      available: date >= todayStart && workDays.includes(date.getDay()) && !booked
    })
  }
  return cells
})
const bookDate = (cell) =>
  openWhatsApp(`Hi Ifiok, I'd like to book a call on ${cell.key}. Are you available?`)

const scrollToProjects = () => {
  document.getElementById('projects')?.scrollIntoView({ behavior: 'smooth' })
}
</script>

<template>
  <div class="if-shell w-full bg-white text-[#12201A]">
    <!-- ============ NAVBAR ============ -->
    <header class="sticky top-0 z-30 bg-white/90 backdrop-blur border-b border-[#EDF2EF]">
      <div class="max-w-7xl mx-auto px-6 lg:px-10 h-16 flex items-center justify-between">
        <div class="flex items-center">
          <img
            src="@/assets/windi-hotspot.png"
            alt="Windi Hotspot"
            class="h-10 w-auto object-contain"
          />
        </div>

        <nav class="hidden md:flex items-center gap-8 text-[14px] text-[#5B6862]">
          <a href="#projects" class="hover:text-[#12201A] transition-colors">Projects</a>
          <a href="#services" class="hover:text-[#12201A] transition-colors">Services</a>
          <a href="#tech" class="hover:text-[#12201A] transition-colors">Technologies</a>
          <a href="#contact" class="hover:text-[#12201A] transition-colors">Contact</a>
        </nav>

        <div class="flex items-center gap-3">
          <a
            href="https://otluaczakfpbcpmpczwq.supabase.co/storage/v1/object/public/windi/IFIOK_USANGA_SOFTWARE_DEVELOPER%20(1).pdf"
            download="Ifiok_Usanga_Software_Developer_Resume.pdf"
            target="_blank"
            rel="noopener noreferrer"
            class="hidden sm:block text-[14px] font-medium text-[#5B6862] hover:text-[#12201A] px-3 py-2 transition-colors"
          >
            Resume
          </a>
          <button
            @click="openWhatsApp('Hi Ifiok, I would like to get in touch with you')"
            class="bg-[#0F9D58] hover:bg-[#0D8A4C] transition-colors text-white text-[14px] font-semibold px-4 py-2.5 rounded-full"
          >
            Get in touch
          </button>
        </div>
      </div>
    </header>

    <!-- ============ HERO ============ -->
    <section class="relative overflow-hidden">
      <div
        class="absolute -top-32 -right-32 h-96 w-96 rounded-full opacity-40 blur-3xl"
        style="background: radial-gradient(circle, #9be8b8 0%, transparent 70%)"
      ></div>

      <div
        class="max-w-7xl mx-auto px-6 lg:px-10 pt-16 pb-20 lg:pt-24 lg:pb-28 grid lg:grid-cols-2 gap-14 items-center relative"
      >
        <div data-aos="fade-right">
          <h1
            class="if-heading text-[38px] sm:text-[48px] lg:text-[56px] font-bold leading-[1.05] mb-5"
          >
            Fintech platforms that scale.
          </h1>
          <p class="text-[16px] lg:text-[17px] text-[#5B6862] leading-relaxed mb-8 max-w-md">
            Senior developer building production-grade lending platforms, payment systems, and
            financial dashboards. From user-facing apps to complex admin analytics.
          </p>
          <div class="flex flex-wrap items-center gap-4">
            <button
              @click="scrollToProjects"
              class="bg-[#0F9D58] hover:bg-[#0D8A4C] transition-colors text-white text-[15px] font-semibold px-6 py-3 rounded-full flex items-center gap-2"
            >
              <i class="mdi mdi-briefcase-outline text-[18px]"></i>
              View my work
            </button>
            <button
              @click="openWhatsApp('Hi Ifiok')"
              class="border border-[#DCE6E0] hover:border-[#0F9D58] hover:text-[#0F9D58] transition-colors text-[15px] font-semibold px-6 py-3 rounded-full"
            >
              Let's talk
            </button>
          </div>
        </div>

        <div data-aos="fade-left" data-aos-delay="150" class="relative h-[340px] lg:h-[400px]">
          <div class="absolute inset-0 rounded-[32px] overflow-hidden bg-[#0F1B14]">
            <img
              :src="profilePic"
              alt="Ifiok Usanga"
              class="h-full w-full object-cover object-top"
            />
            <div
              class="absolute inset-0 bg-gradient-to-t from-[#0F1B14]/70 via-transparent to-transparent"
            ></div>
          </div>
        </div>
      </div>
    </section>

    <!-- ============ STATS STRIP ============ -->
    <section class="border-y border-[#EDF2EF] bg-[#F8FBF9]">
      <div class="max-w-7xl mx-auto px-6 lg:px-10 py-10 grid grid-cols-2 lg:grid-cols-4 gap-8">
        <div
          v-for="(s, i) in stats"
          :key="s.label"
          data-aos="zoom-in"
          :data-aos-delay="i * 100"
          class="text-center lg:text-left"
        >
          <p class="if-heading text-[28px] lg:text-[32px] font-bold text-[#0F9D58]">
            {{ s.value }}
          </p>
          <p class="text-[13px] text-[#5B6862]">{{ s.label }}</p>
        </div>
      </div>
    </section>

    <!-- ============ SERVICES ============ -->
    <section id="services" class="max-w-7xl mx-auto px-6 lg:px-10 py-20 lg:py-28">
      <div class="text-center max-w-xl mx-auto mb-14" data-aos="fade-up">
        <h2 class="if-heading text-[30px] lg:text-[36px] font-bold mb-3">What I build</h2>
        <p class="text-[15px] text-[#5B6862] leading-relaxed">
          End-to-end development expertise. From architectural planning to deployment, I deliver
          solutions that scale.
        </p>
      </div>

      <div class="grid md:grid-cols-2 gap-6">
        <div
          v-for="(service, i) in services"
          :key="service.title"
          data-aos="fade-up"
          :data-aos-delay="i * 120"
          class="rounded-2xl border border-[#EDF2EF] hover:border-[#0F9D58] hover:shadow-lg transition-all p-6 lg:p-7"
        >
          <div
            class="h-11 w-11 rounded-xl bg-[#EAF7EF] flex items-center justify-center mb-4 shrink-0"
          >
            <i class="mdi text-[#0F9D58] text-[20px]" :class="service.icon"></i>
          </div>
          <p class="text-[15px] font-semibold mb-2">{{ service.title }}</p>
          <p class="text-[13.5px] text-[#5B6862] leading-relaxed">{{ service.copy }}</p>
        </div>
      </div>
    </section>

    <!-- ============ FEATURED PROJECTS ============ -->
    <section id="projects" class="bg-[#F8FBF9] py-20 lg:py-28">
      <div class="max-w-7xl mx-auto px-6 lg:px-10">
        <div class="flex items-end justify-between mb-10" data-aos="fade-up">
          <div>
            <h2 class="if-heading text-[30px] lg:text-[36px] font-bold mb-2">Featured work</h2>
            <p class="text-[15px] text-[#5B6862]">
              High-impact projects across web, mobile, and AI.
            </p>
          </div>
          <a
            href="#"
            class="hidden sm:flex items-center gap-1 text-[14px] font-semibold text-[#0F9D58] hover:underline"
          >
            See all work
            <i class="mdi mdi-chevron-right"></i>
          </a>
        </div>

        <div class="grid sm:grid-cols-2 lg:grid-cols-3 gap-6">
          <component
            :is="project.link ? 'a' : 'div'"
            v-for="(project, i) in featuredProjects"
            :key="project.id"
            :href="project.link || undefined"
            target="_blank"
            rel="noopener noreferrer"
            data-aos="fade-up"
            :data-aos-delay="(i % 3) * 100"
            class="bg-white rounded-2xl border border-[#EDF2EF] overflow-hidden hover:shadow-lg hover:-translate-y-1 transition-all group block"
          >
            <div class="relative aspect-[16/10] bg-[#EAF7EF] overflow-hidden">
              <!-- live site preview -->
              <iframe
                v-if="project.link && project.embed !== false"
                :src="project.link"
                :title="project.title"
                loading="lazy"
                tabindex="-1"
                sandbox="allow-scripts allow-same-origin"
                class="absolute top-0 left-0 w-[400%] h-[400%] origin-top-left scale-[0.25] pointer-events-none border-0 bg-white"
              ></iframe>

              <!-- fallback image (for sites that block embedding) -->
              <img
                v-else-if="project.image"
                :src="project.image"
                :alt="project.title"
                loading="lazy"
                class="h-full w-full object-cover object-top"
              />

              <div v-else class="h-full w-full flex items-center justify-center">
                <i class="mdi mdi-code-braces text-[#0F9D58] text-[40px]"></i>
              </div>

              <span
                v-if="project.link"
                class="absolute top-3 right-3 h-8 w-8 rounded-full bg-white/90 flex items-center justify-center opacity-0 group-hover:opacity-100 transition-opacity"
              >
                <i class="mdi mdi-open-in-new text-[#0F9D58] text-[16px]"></i>
              </span>
            </div>
            <div class="p-5">
              <p class="text-[15px] font-semibold mb-1">{{ project.title }}</p>
              <p class="text-[12.5px] text-[#9AA79E] mb-4 line-clamp-2">
                {{ project.description }}
              </p>
            </div>
          </component>
        </div>
      </div>
    </section>

    <!-- ============ TECHNOLOGIES ============ -->
    <section id="tech" class="max-w-7xl mx-auto px-6 lg:px-10 py-20 lg:py-28">
      <div class="text-center max-w-xl mx-auto mb-12" data-aos="fade-up">
        <h2 class="if-heading text-[30px] lg:text-[36px] font-bold mb-3">Tech stack</h2>
        <p class="text-[15px] text-[#5B6862] leading-relaxed">
          20+ technologies mastered across frontend, backend, mobile, and AI.
        </p>
      </div>

      <div class="grid grid-cols-2 sm:grid-cols-4 gap-4">
        <div
          v-for="(g, i) in technologies"
          :key="g.name"
          data-aos="fade-up"
          :data-aos-delay="(i % 4) * 80"
          class="rounded-2xl border border-[#EDF2EF] hover:border-[#0F9D58] transition-colors p-5 text-center cursor-pointer"
        >
          <p class="text-[15px] font-semibold mb-1">{{ g.name }}</p>
          <p class="text-[12px] text-[#9AA79E]">{{ g.level }}</p>
        </div>
      </div>
    </section>

    <!-- ============ TESTIMONIALS ============ -->
    <section class="max-w-7xl mx-auto px-6 lg:px-10 py-20 lg:py-28">
      <div class="text-center max-w-xl mx-auto mb-14" data-aos="fade-up">
        <h2 class="if-heading text-[30px] lg:text-[36px] font-bold mb-3">Trusted by teams</h2>
        <p class="text-[15px] text-[#5B6862]">What clients say about working together.</p>
      </div>

      <div class="grid md:grid-cols-3 gap-6">
        <div
          v-for="(t, i) in testimonials"
          :key="t.name"
          data-aos="fade-up"
          :data-aos-delay="i * 120"
          class="rounded-2xl border border-[#EDF2EF] p-6 flex flex-col"
        >
          <i class="fa-solid fa-quote-left text-[#9BE8B8] text-xl mb-4"></i>
          <p class="text-[14px] text-[#3C4A43] leading-relaxed mb-6 flex-1">{{ t.quote }}</p>
          <div class="flex items-center gap-3">
            <div
              class="h-9 w-9 rounded-full bg-[#0F9D58] flex items-center justify-center text-white text-[12px] font-semibold"
            >
              {{
                t.name
                  .split(' ')
                  .map((n) => n[0])
                  .join('')
              }}
            </div>
            <div>
              <p class="text-[13px] font-semibold">{{ t.name }}</p>
              <p class="text-[11.5px] text-[#9AA79E]">{{ t.role }}</p>
            </div>
          </div>
        </div>
      </div>
    </section>

    <section id="availability" class="max-w-7xl mx-auto px-6 lg:px-10 py-20 lg:py-28">
      <div class="text-center max-w-xl mx-auto mb-12" data-aos="fade-up">
        <h2 class="if-heading text-[30px] lg:text-[36px] font-bold mb-3">My availability</h2>
        <p class="text-[15px] text-[#5B6862]">
          Pick an open date and message me on WhatsApp to book a call.
        </p>
      </div>

      <div data-aos="fade-up" class="max-w-md mx-auto rounded-3xl border border-[#EDF2EF] p-6">
        <div class="flex items-center justify-between mb-5">
          <button
            @click="changeMonth(-1)"
            :disabled="!canGoBack"
            class="h-9 w-9 rounded-full hover:bg-[#F5F9F6] disabled:opacity-30"
          >
            <i class="mdi mdi-chevron-left"></i>
          </button>
          <p class="if-heading font-bold">{{ monthLabel }}</p>
          <button @click="changeMonth(1)" class="h-9 w-9 rounded-full hover:bg-[#F5F9F6]">
            <i class="mdi mdi-chevron-right"></i>
          </button>
        </div>

        <div class="grid grid-cols-7 gap-1 text-center text-[11.5px] text-[#9AA79E] mb-2">
          <span v-for="d in ['S', 'M', 'T', 'W', 'T', 'F', 'S']" :key="d + Math.random()">{{
            d
          }}</span>
        </div>

        <div class="grid grid-cols-7 gap-1">
          <template v-for="(c, i) in calendarCells" :key="i">
            <div v-if="!c"></div>
            <button
              v-else
              :disabled="!c.available"
              @click="bookDate(c)"
              class="aspect-square rounded-xl text-[13px] font-medium transition-colors"
              :class="
                c.available
                  ? 'bg-[#EAF7EF] text-[#0F9D58] hover:bg-[#0F9D58] hover:text-white'
                  : c.booked
                    ? 'bg-[#FDECEC] text-[#D64545] line-through cursor-not-allowed'
                    : 'text-[#C5CFC9] cursor-not-allowed'
              "
            >
              {{ c.d }}
            </button>
          </template>
        </div>

        <div class="flex items-center justify-center gap-5 mt-5 text-[12px] text-[#5B6862]">
          <span class="flex items-center gap-1.5"
            ><i class="h-2.5 w-2.5 rounded-full bg-[#0F9D58]"></i>Available</span
          >
          <span class="flex items-center gap-1.5"
            ><i class="h-2.5 w-2.5 rounded-full bg-[#D64545]"></i>Booked</span
          >
        </div>
      </div>
    </section>

    <!-- ============ ENGAGEMENT OPTIONS ============ -->
    <section class="bg-[#F8FBF9] py-20 lg:py-28">
      <div class="max-w-7xl mx-auto px-6 lg:px-10">
        <div class="text-center max-w-xl mx-auto mb-14" data-aos="fade-up">
          <h2 class="if-heading text-[30px] lg:text-[36px] font-bold mb-3">How we can work</h2>
          <p class="text-[15px] text-[#5B6862] leading-relaxed">
            Flexible engagement models for projects of any scale.
          </p>
        </div>

        <div class="grid md:grid-cols-3 gap-6">
          <div
            v-for="(plan, i) in plans"
            :key="plan.name"
            data-aos="fade-up"
            :data-aos-delay="i * 120"
            class="rounded-3xl p-7 flex flex-col"
            :class="plan.tag ? 'bg-[#0F9D58]' : 'bg-white border border-[#EDF2EF]'"
          >
            <span
              v-if="plan.tag"
              class="self-start text-[11px] font-semibold text-[#0F1B14] bg-white rounded-full px-3 py-1 mb-4"
              >{{ plan.tag }}</span
            >
            <p
              class="if-heading text-[18px] font-bold mb-1"
              :class="plan.tag ? 'text-white' : 'text-[#12201A]'"
            >
              {{ plan.name }}
            </p>
            <div class="flex items-baseline gap-1 mb-6">
              <span
                class="text-[34px] font-bold"
                :class="plan.tag ? 'text-white' : 'text-[#0F9D58]'"
                >{{ plan.price }}</span
              >
              <span class="text-[13px]" :class="plan.tag ? 'text-white/80' : 'text-[#9AA79E]'">{{
                plan.period
              }}</span>
            </div>
            <ul class="flex flex-col gap-3 mb-8 flex-1">
              <li
                v-for="f in plan.features"
                :key="f"
                class="flex items-start gap-2 text-[13.5px]"
                :class="plan.tag ? 'text-white/90' : 'text-[#5B6862]'"
              >
                <i
                  class="mdi mdi-check-circle text-[16px] mt-0.5 shrink-0"
                  :class="plan.tag ? 'text-white' : 'text-[#4ADE80]'"
                ></i>
                {{ f }}
              </li>
            </ul>
            <button
              @click="contactOnWhatsApp(plan.name)"
              class="w-full py-3 rounded-full text-[14px] font-semibold transition-colors"
              :class="
                plan.tag
                  ? 'bg-white text-[#0F9D58] hover:bg-white/90'
                  : 'bg-white/10 text-[#12201A] hover:bg-white/20'
              "
            >
              {{ plan.cta }}
            </button>
          </div>
        </div>
      </div>
    </section>

    <!-- ============ CTA BANNER ============ -->
    <section class="max-w-7xl mx-auto px-6 lg:px-10 pb-20 lg:pb-28">
      <div
        data-aos="zoom-in"
        class="relative overflow-hidden rounded-[32px] bg-[#0F1B14] px-8 py-14 lg:py-16 text-center"
      >
        <div
          class="absolute inset-0 opacity-90"
          style="background: radial-gradient(circle at 20% 20%, #1f5e3c 0%, #0f1b14 65%)"
        ></div>
        <div class="relative z-10 max-w-lg mx-auto">
          <h2 class="if-heading text-white text-[28px] lg:text-[34px] font-bold mb-4">
            Ready to build something great?
          </h2>
          <p class="text-[#B7C4BC] text-[15px] mb-8">
            Let's collaborate. Whether it's a consultation, a full project, or ongoing support — I'm
            ready.
          </p>
          <div class="flex flex-wrap justify-center gap-4">
            <button
              @click="
                openWhatsApp(
                  'Hi Ifiok, I would like to start a project with you. I’d love to discuss the requirements and next steps.'
                )
              "
              class="bg-[#0F9D58] hover:bg-[#0D8A4C] transition-colors text-white text-[15px] font-semibold px-6 py-3 rounded-full"
            >
              Start a project
            </button>
            <button
              @click="
                openWhatsApp(
                  'Hi Ifiok, I would like to schedule a call to discuss a potential project.'
                )
              "
              class="border border-white/20 hover:border-white/40 transition-colors text-white text-[15px] font-semibold px-6 py-3 rounded-full"
            >
              Schedule a call
            </button>
          </div>
        </div>
      </div>
    </section>

    <!-- ============ FOOTER ============ -->
    <footer class="border-t border-[#EDF2EF]">
      <div class="max-w-7xl mx-auto px-6 lg:px-10 py-14 grid sm:grid-cols-2 lg:grid-cols-5 gap-10">
        <div class="lg:col-span-2">
          <div class="flex items-center">
            <img
              src="@/assets/windi-hotspot.png"
              alt="Windi Hotspot"
              class="h-10 w-auto object-contain"
            />
          </div>
          <p class="text-[13.5px] text-[#5B6862] leading-relaxed mb-5 max-w-xs">
            Senior software developer building scalable, innovative solutions across web, mobile,
            and AI. Let's create something that matters.
          </p>
          <div class="flex items-center gap-3">
            <a
              href="https://www.linkedin.com/in/ifiok-usanga/"
              class="h-9 w-9 rounded-full bg-[#F5F9F6] flex items-center justify-center text-[#5B6862] hover:text-[#0F9D58]"
            >
              <i class="fa-brands fa-linkedin text-xs"></i>
            </a>
            <a
              href="https://github.com/Lucy Kim"
              class="h-9 w-9 rounded-full bg-[#F5F9F6] flex items-center justify-center text-[#5B6862] hover:text-[#0F9D58]"
            >
              <i class="fa-brands fa-github text-xs"></i>
            </a>
            <a
              href="https://x.com/boiwindi?s=11"
              class="h-9 w-9 rounded-full bg-[#F5F9F6] flex items-center justify-center text-[#5B6862] hover:text-[#0F9D58]"
            >
              <i class="fa-brands fa-x-twitter text-xs"></i>
            </a>
            <a
              href="mailto:ifokusanga69@gmail.com"
              class="h-9 w-9 rounded-full bg-[#F5F9F6] flex items-center justify-center text-[#5B6862] hover:text-[#0F9D58]"
            >
              <i class="fa-solid fa-envelope text-xs"></i>
            </a>
          </div>
        </div>

        <div v-for="(links, section) in footerLinks" :key="section">
          <p class="text-[13px] font-semibold mb-4 text-[#12201A]">{{ section }}</p>
          <ul class="flex flex-col gap-3">
            <li v-for="link in links" :key="link">
              <a href="#" class="text-[13.5px] text-[#5B6862] hover:text-[#0F9D58]">{{ link }}</a>
            </li>
          </ul>
        </div>
      </div>

      <div class="border-t border-[#EDF2EF]">
        <div
          class="max-w-7xl mx-auto px-6 lg:px-10 py-5 flex flex-col sm:flex-row items-center justify-between gap-2"
        >
          <p class="text-[12.5px] text-[#9AA79E]">© 2026 Ifiok Usanga. All rights reserved.</p>
          <p class="text-[12.5px] text-[#9AA79E]">Designed & developed for simplicity.</p>
        </div>
      </div>
    </footer>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Manrope:wght@600;700;800&family=Inter:wght@400;500;600&display=swap');

.if-shell {
  font-family: 'Inter', ui-sans-serif, system-ui, sans-serif;
}

.if-heading {
  font-family: 'Manrope', ui-sans-serif, system-ui, sans-serif;
  letter-spacing: -0.01em;
}
</style>
