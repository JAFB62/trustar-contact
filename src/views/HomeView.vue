<script setup lang="ts">
import { computed, ref, watch } from 'vue'

type Language = 'pt-BR' | 'en'

const language = ref<Language>('pt-BR')

const content = {
  'pt-BR': {
    languageLabel: 'Selecionar idioma',
    eyebrow: 'Segurança que entende o contexto',
    titlePrefix: 'Conheça o',
    productName: 'trustAR',
    titleHighlight: 'Comunicação confiável.',
    videoLabel: 'Vídeo de apresentação do trustAR',
    videoUnsupported: 'Seu navegador não suporta a reprodução de vídeos.',
    sectionEyebrow: 'Por que trustAR',
    sectionTitle:
      'Camada inteligente que conecta identidade, risco e decisão em experiências digitais mais seguras e fluidas.',
    footer: 'Vamos conversar sobre confiança digital.',
    email: 'E-mail',
    differentials: [
      {
        title: 'Camada de Comunicação Confiável',
        description: 'Uma nova camada de confiança entre autenticação e autorização.',
      },
      {
        title: 'Integração por API',
        description: 'Compatível com provedores de identidade e motores de risco existentes.',
      },
      {
        title: 'Validação Contextual Adaptativa',
        description: 'Acionada apenas quando o risco justificar.',
      },
      {
        title: 'Pronto para Open Finance e IA',
        description:
          'Arquitetura preparada para evoluir com novos sinais de contexto e inteligência artificial.',
      },
    ],
  },
  en: {
    languageLabel: 'Select language',
    eyebrow: 'Security that understands context',
    titlePrefix: 'Meet',
    productName: 'trustAR',
    titleHighlight: 'Reliable communication.',
    videoLabel: 'trustAR presentation video',
    videoUnsupported: 'Your browser does not support video playback.',
    sectionEyebrow: 'Why trustAR',
    sectionTitle:
      'An intelligent layer that connects identity, risk, and decision-making for safer, smoother digital experiences.',
    footer: "Let's talk about digital trust.",
    email: 'Email',
    differentials: [
      {
        title: 'Trusted Communication Layer',
        description: 'A new layer of trust between authentication and authorization.',
      },
      {
        title: 'API Integration',
        description: 'Compatible with existing identity providers and risk engines.',
      },
      {
        title: 'Adaptive Contextual Validation',
        description: 'Triggered only when risk justifies it.',
      },
      {
        title: 'Ready for Open Finance and AI',
        description:
          'An architecture ready to evolve with new context signals and artificial intelligence.',
      },
    ],
  },
} as const

const icons = ['shield', 'nodes', 'pulse', 'spark'] as const
const copy = computed(() => content[language.value])
const publicBase = import.meta.env.BASE_URL.endsWith('/')
  ? import.meta.env.BASE_URL
  : `${import.meta.env.BASE_URL}/`
const assetPath = (path: string) => `${publicBase}${path}`
const logoSource = assetPath('img/trustar-logo.svg')
const videoPoster = assetPath('favicon.ico')
const videoSource = computed(() =>
  language.value === 'en'
    ? assetPath('video/pitch_trustar_fintech2026-en.mp4')
    : assetPath('video/pitch_trustar_fintech2026.mp4'),
)
const differentials = computed(() =>
  copy.value.differentials.map((item, index) => ({
    ...item,
    number: String(index + 1).padStart(2, '0'),
    icon: icons[index],
  })),
)

watch(
  language,
  (value) => {
    document.documentElement.lang = value === 'pt-BR' ? 'pt-BR' : 'en'
  },
  { immediate: true },
)
</script>

<template>
  <main class="landing-page">
    <section class="hero" aria-labelledby="hero-title">
      <div class="ambient-shape ambient-shape--one"></div>
      <div class="ambient-shape ambient-shape--two"></div>

      <div class="hero__inner">
        <div class="hero__topbar">
          <img class="brand" :src="logoSource" alt="trustAR" />
          <div class="language-switcher" :aria-label="copy.languageLabel">
            <button :class="{ 'is-active': language === 'pt-BR' }" :aria-pressed="language === 'pt-BR'" type="button"
              @click="language = 'pt-BR'">PT</button>
            <span aria-hidden="true">/</span>
            <button :class="{ 'is-active': language === 'en' }" :aria-pressed="language === 'en'" type="button"
              @click="language = 'en'">EN</button>
          </div>
        </div>

        <p class="eyebrow">{{ copy.eyebrow }}</p>
        <h1 id="hero-title">
          <span class="title-prefix">{{ copy.titlePrefix }}</span>
          <span class="title-product">{{ copy.productName }}</span>
          <em>{{ copy.titleHighlight }}</em>
        </h1>
        <p class="hero__intro">

        </p>

        <div class="video-frame">
          <video :key="language" controls preload="metadata" :poster="videoPoster" :aria-label="copy.videoLabel">
            <source :src="videoSource" type="video/mp4" />
            {{ copy.videoUnsupported }}
          </video>
        </div>
      </div>
    </section>

    <section class="differentials" aria-labelledby="differentials-title">
      <div class="section-heading">
        <p class="eyebrow">{{ copy.sectionEyebrow }}</p>
        <h2 id="differentials-title">{{ copy.sectionTitle }}</h2>
      </div>

      <div class="differentials__grid">
        <article v-for="item in differentials" :key="item.number" class="differential-card">
          <div class="card-topline">
            <span class="icon-wrap" :class="`icon-wrap--${item.icon}`" aria-hidden="true">
              <svg v-if="item.icon === 'shield'" viewBox="0 0 24 24" fill="none">
                <path d="M12 3.5 19 6v5.25c0 4.25-2.88 7.94-7 9.25-4.12-1.31-7-5-7-9.25V6l7-2.5Z" />
                <path d="m8.75 11.9 2.1 2.1 4.45-4.45" />
              </svg>
              <svg v-else-if="item.icon === 'nodes'" viewBox="0 0 24 24" fill="none">
                <circle cx="6" cy="12" r="2.25" />
                <circle cx="18" cy="6" r="2.25" />
                <circle cx="18" cy="18" r="2.25" />
                <path d="m8 11 7.9-4M8 13l7.9 4" />
              </svg>
              <svg v-else-if="item.icon === 'pulse'" viewBox="0 0 24 24" fill="none">
                <path d="M3.5 12h3.1l1.9-5 3.2 10 2.2-6h6.6" />
              </svg>
              <svg v-else viewBox="0 0 24 24" fill="none">
                <path d="M12 3.5 13.6 9l5.4 1.6-5.4 1.6-1.6 5.4-1.6-5.4L5 10.6 10.4 9 12 3.5Z" />
                <path d="m18.5 15 .7 2.3 2.3.7-2.3.7-.7 2.3-.7-2.3-2.3-.7 2.3-.7.7-2.3Z" />
              </svg>
            </span>
            <span class="card-number">{{ item.number }}</span>
          </div>
          <h3>{{ item.title }}</h3>
          <p>{{ item.description }}</p>
        </article>
      </div>
    </section>
  </main>

  <footer class="footer">
    <div class="footer__inner">
      <p>{{ copy.footer }}</p>
      <div class="footer__contacts">
        <a href="https://wa.me/5551996405858" target="_blank" rel="noopener noreferrer">
          WhatsApp <span>(51) 99640-5858</span>
        </a>
        <a href="mailto:joelafbarbosa@gmail.com">
          {{ copy.email }} <span>joelafbarbosa@gmail.com</span>
        </a>
      </div>
    </div>
  </footer>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=DM+Mono:wght@400;500&family=Manrope:wght@400;500;600;700;800&family=Playfair+Display:ital,wght@0,600;0,700;1,600;1,700&display=swap');

:global(*) {
  box-sizing: border-box;
}

:global(body) {
  margin: 0;
  background: #f7f8f7;
  color: #102923;
  font-family: 'Manrope', sans-serif;
}

.landing-page {
  min-height: 100vh;
  overflow: hidden;
}

.hero {
  position: relative;
  isolation: isolate;
  background: #0b2923;
  color: #f7f4ed;
  padding: 34px 24px 96px;
  overflow: hidden;
}

.hero::after {
  content: '';
  position: absolute;
  z-index: -1;
  right: -14vw;
  bottom: -28vw;
  width: 58vw;
  aspect-ratio: 1;
  border: 1px solid rgba(181, 221, 204, .14);
  border-radius: 50%;
  box-shadow: 0 0 0 76px rgba(181, 221, 204, .04), 0 0 0 152px rgba(181, 221, 204, .025);
}

.hero__inner,
.differentials {
  width: min(1120px, 100%);
  margin: 0 auto;
}

.brand {
  display: block;
  width: 146px;
  height: auto;
}

.hero__topbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 24px;
}

.language-switcher {
  display: inline-flex;
  align-items: center;
  gap: 7px;
  color: #78a88f;
  font: 500 12px 'DM Mono', monospace;
  letter-spacing: .08em;
}

.language-switcher button {
  padding: 4px 0;
  border: 0;
  background: transparent;
  color: inherit;
  cursor: pointer;
  font: inherit;
  letter-spacing: inherit;
}

.language-switcher button:hover,
.language-switcher button.is-active {
  color: #f7f4ed;
}

.eyebrow {
  margin: 50px 0 10px;
  color: #a4d7ba;
  font: 500 11px/1.3;
  /*'DM Mono', monospace; */
  letter-spacing: .08em;
  text-transform: uppercase;
}

.hero .eyebrow {
  color: #c0ebcf;
  font-size: 18px;
  font-weight: 500;
  letter-spacing: .12em;
}

h1 {
  max-width: 790px;
  margin: 0;
  font-size: clamp(42px, 6.1vw, 78px);
  font-weight: 700;
  line-height: 1.04;
  letter-spacing: -.07em;
}

.title-prefix {
  font-size: .62em;
  letter-spacing: -.05em;
}

.title-product {
  color: #f7f4ed;
  padding-left: 20px;
}

h1 em {
  display: block;
  color: #b6dfc8;
  /* font-family: 'Playfair Display', Georgia, serif; */
  font-size: .52em;
  font-weight: 600;
  letter-spacing: -.05em;
}

.hero__intro {
  max-width: 550px;
  margin: 17px 0 25px;
  color: #c1d1cb;
  font-size: 16px;
  line-height: 1.7;
}

.video-frame {
  position: relative;
  width: min(900px, 100%);
  margin: 0 auto;
  padding: 7px;
  border: 1px solid rgba(202, 235, 217, .25);
  border-radius: 12px;
  background: rgba(255, 255, 255, .08);
  box-shadow: 0 27px 60px rgba(0, 0, 0, .25);
}

.video-frame::before {
  content: '';
  position: absolute;
  top: -1px;
  left: 23%;
  width: 30%;
  height: 1px;
  background: #a4d7ba;
}

video {
  display: block;
  width: 100%;
  aspect-ratio: 16 / 9;
  border-radius: 6px;
  background: #071915;
  object-fit: contain;
}

.ambient-shape {
  position: absolute;
  z-index: -1;
  border-radius: 50%;
  filter: blur(1px);
}

.ambient-shape--one {
  top: 150px;
  right: 9vw;
  width: 13px;
  height: 13px;
  background: #9ed8ba;
  box-shadow: 0 0 0 13px rgba(158, 216, 186, .1), 0 0 0 31px rgba(158, 216, 186, .05);
}

.ambient-shape--two {
  left: -66px;
  top: 49%;
  width: 180px;
  height: 180px;
  border: 1px solid rgba(179, 221, 198, .14);
}

.differentials {
  padding: 94px 24px 108px;
}

.section-heading {
  display: flex;
  justify-content: space-between;
  align-items: end;
  gap: 35px;
  margin-bottom: 40px;
}

.section-heading .eyebrow {
  margin: 0 0 14px;
  color: #3c8a6b;
}

h2 {
  margin: 0;
  font-size: clamp(30px, 4vw, 48px);
  line-height: 1.1;
  letter-spacing: -.06em;
}

.differentials__grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 14px;
}

.differential-card {
  min-height: 274px;
  padding: 23px;
  border: 1px solid #dbe3de;
  border-radius: 10px;
  background: #fff;
  box-shadow: 0 8px 24px rgba(14, 44, 35, .035);
  transition: transform .2s ease, box-shadow .2s ease;
}

.differential-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 14px 30px rgba(14, 44, 35, .09);
}

.card-topline {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 44px;
}

.icon-wrap {
  display: grid;
  place-items: center;
  width: 41px;
  height: 41px;
  border-radius: 50%;
  background: #e8f3ed;
  color: #27745b;
}

.icon-wrap svg {
  width: 22px;
  height: 22px;
  stroke: currentColor;
  stroke-width: 1.5;
  stroke-linecap: round;
  stroke-linejoin: round;
}

.icon-wrap--pulse {
  background: #ecf3e2;
  color: #607d3a;
}

.icon-wrap--spark {
  background: #f6efe0;
  color: #a77931;
}

.card-number {
  color: #9aa9a3;
  font: 500 11px 'DM Mono', monospace;
  letter-spacing: .08em;
}

h3 {
  margin: 0 0 12px;
  color: #16372e;
  font-size: 18px;
  line-height: 1.32;
  letter-spacing: -.035em;
}

.differential-card p {
  margin: 0;
  color: #63736d;
  font-size: 13px;
  line-height: 1.65;
}

.footer {
  background: #0b2923;
  color: #f7f4ed;
  padding: 32px 24px;
}

.footer__inner {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 24px;
  width: min(1120px, 100%);
  margin: 0 auto;
}

.footer p {
  margin: 0;
  font-size: 14px;
  font-weight: 600;
}

.footer__contacts {
  display: flex;
  flex-wrap: wrap;
  gap: 24px;
}

.footer a {
  color: #a4d7ba;
  font: 500 12px 'DM Mono', monospace;
  letter-spacing: .03em;
  text-decoration: none;
}

.footer a:hover {
  color: #f7f4ed;
}

.footer a span {
  color: #f7f4ed;
}

@media (max-width: 820px) {
  .differentials__grid {
    grid-template-columns: repeat(2, 1fr);
  }

  .section-heading {
    display: block;
  }

  .section-heading h2 {
    max-width: 600px;
  }
}

@media (max-width: 560px) {
  .hero {
    padding: 24px 18px 68px;
  }

  .language-switcher {
    font-size: 11px;
  }

  .eyebrow {
    margin-top: 40px;
  }

  .hero__intro {
    font-size: 14px;
  }

  .differentials {
    padding: 66px 18px 75px;
  }

  .differentials__grid {
    grid-template-columns: 1fr;
  }

  .footer {
    padding: 26px 18px;
  }

  .footer__inner {
    align-items: flex-start;
    flex-direction: column;
    gap: 16px;
  }

  .footer__contacts {
    flex-direction: column;
    gap: 10px;
  }

  .differential-card {
    min-height: auto;
  }

  .card-topline {
    margin-bottom: 28px;
  }
}
</style>
