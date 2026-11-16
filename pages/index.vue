<script setup lang="ts">
import { computed, onMounted, onUnmounted, ref } from 'vue'

const weddingDate = new Date('2027-06-12T15:00:00')
const now = ref(new Date())
let timer: ReturnType<typeof setInterval>
let revealObserver: IntersectionObserver | undefined
onMounted(() => {
  timer = setInterval(() => now.value = new Date(), 1000)
  if ('IntersectionObserver' in window) {
    const targets = document.querySelectorAll('.intro, .story-copy, .story .image-circle, .section-heading, .event, .quote-photo, .quote-copy, .info-grid article, .rsvp-section > *')
    targets.forEach((el) => el.classList.add('will-reveal'))
    revealObserver = new IntersectionObserver((entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add('is-visible')
          revealObserver?.unobserve(entry.target)
        }
      })
    }, { threshold: 0.14, rootMargin: '0px 0px -35px 0px' })
    targets.forEach((el) => revealObserver?.observe(el))
  }
})
onUnmounted(() => {
  clearInterval(timer)
  revealObserver?.disconnect()
})
const countdown = computed(() => {
  const ms = Math.max(0, weddingDate.getTime() - now.value.getTime())
  return [
    Math.floor(ms / 86400000),
    Math.floor(ms % 86400000 / 3600000),
    Math.floor(ms % 3600000 / 60000),
    Math.floor(ms % 60000 / 1000)
  ]
})
const guestName = ref('')
const guestEmail = ref('')
const attendance = ref('oui')
const guestCount = ref('2')
const guestCountOpen = ref(false)
const message = ref('')
const submitted = ref(false)
function submitRsvp() {
  if (!guestName.value.trim() || !guestEmail.value.trim()) return
  submitted.value = true
}
</script>

<template>
  <main>
    <header class="topbar">
      <a class="monogram" href="#home">S<span>&</span>D</a>
      <nav>
        <a href="#histoire">Notre histoire</a>
        <a href="#journee">La journée</a>
        <a href="#infos">Les informations</a>
      </nav>
      <a class="nav-rsvp" href="#rsvp">Répondre à l'invitation <span>↗</span></a>
    </header>

    <section id="home" class="hero">
      <div class="hero-copy">
        <p class="eyebrow">LE BONHEUR DE VOUS AVOIR À NOS CÔTÉS</p>
        <p class="script pretitle">Nous nous marions…</p>
        <h1>Sarah <em>&</em><br>David</h1>
        <div class="ornament"><span></span> ✿ <span></span></div>
        <p class="hero-date">SAMEDI 12 JUIN 2027 <i>·</i> PARIS</p>
        <a href="#histoire" class="round-link">Découvrir<br>notre journée <b>↓</b></a>
      </div>
      <div class="hero-photo-wrap">
        <div class="photo-halo"></div>
        <div class="hero-photo"></div>
        <span class="photo-caption">UN GRAND OUI, POUR LA VIE</span>
        <span class="flower flower-one">✿</span>
        <span class="flower flower-two">❀</span>
      </div>
      <div class="hero-bottom">12 . 06 . 2027 <span>AVEC TOUT NOTRE AMOUR</span> PARIS, FRANCE</div>
    </section>

    <section class="intro section-pad">
      <div class="feature-legend"><span><i class="feature-dot included"></i> Inclus dans la formule</span><span><i class="feature-dot optional"></i> Option en supplément</span></div>
      <span class="feature-badge included">Inclus dans la formule</span>
      <p class="script">Une belle histoire, un nouveau chapitre</p>
      <h2>Et si nous célébrions<br><em>l'amour ensemble ?</em></h2>
      <p class="intro-text">Après tant de jolis souvenirs, nous avons hâte de commencer cette nouvelle aventure entourés des personnes qui comptent le plus pour nous. Votre présence rendra cette journée encore plus belle.</p>
      <div class="tiny-flower">✿</div>
    </section>

    <section id="histoire" class="story section-pad">
      <div class="story-image image-circle"></div>
      <div class="story-copy">
        <p class="eyebrow">DEUX CŒURS, UNE HISTOIRE</p>
        <p class="script">Tout a commencé par…</p>
        <h2>Une rencontre,<br><em>mille émotions.</em></h2>
        <p>Un regard, un sourire, des conversations qui n'en finissent pas… Puis l'évidence. Aujourd'hui, nous avons envie de vous raconter la suite, entourés de ceux que nous aimons.</p>
        <span class="signature">Sarah & David</span>
      </div>
    </section>

    <section class="countdown-section">
      <span class="feature-badge optional">Option</span>
      <p class="script">Le grand jour approche</p>
      <h2>Plus que quelques <em>instants…</em></h2>
      <div class="countdown">
        <div v-for="(value, index) in countdown" :key="index"><strong>{{ String(value).padStart(2, '0') }}</strong><span>{{ ['JOURS','HEURES','MINUTES','SECONDES'][index] }}</span></div>
      </div>
      <p class="countdown-note">Nous avons tellement hâte de vous retrouver !</p>
      <div class="count-flower">❀</div>
    </section>

    <section id="journee" class="day-section section-pad">
      <div class="section-heading">
        <span class="feature-badge optional">Option</span>
        <p class="eyebrow">À VOS AGENDAS</p>
        <p class="script">Le samedi 12 juin 2027</p>
        <h2>Le programme de<br><em>notre journée</em></h2>
      </div>
      <div class="timeline">
        <article class="event">
          <div class="event-photo event-one"></div>
          <div class="event-info"><span class="event-time">15H00</span><h3>La cérémonie</h3><p>Le début de notre plus belle aventure, à partager tous ensemble.</p><small>JARDIN DES ÉTOILES · PARIS</small></div>
        </article>
        <article class="event reverse">
          <div class="event-photo event-two"></div>
          <div class="event-info"><span class="event-time">17H00</span><h3>Le cocktail</h3><p>Quelques bulles, de belles retrouvailles et beaucoup de joie.</p><small>JARDIN DES ÉTOILES · PARIS</small></div>
        </article>
        <article class="event">
          <div class="event-photo event-three"></div>
          <div class="event-info"><span class="event-time">20H00</span><h3>Le dîner & la fête</h3><p>Une soirée pleine de surprises, de musique et de danse.</p><small>JARDIN DES ÉTOILES · PARIS</small></div>
        </article>
      </div>
    </section>

    <section class="quote-section">
      <div class="quote-photo"></div>
      <div class="quote-copy"><span>“</span><p class="script">Le plus beau voyage est celui que l'on fait à deux.</p><small>ET LE NÔTRE NE FAIT QUE COMMENCER</small></div>
    </section>

    <section id="infos" class="info-section section-pad">
      <p class="eyebrow">POUR PROFITER PLEINEMENT</p>
      <p class="script">Quelques petits détails</p>
      <h2>Tout ce qu'il faut <em>savoir</em></h2>
      <div class="info-grid">
        <article><span class="feature-badge included">Inclus</span><div class="info-icon">⌖</div><h3>Le lieu</h3><p>Jardin des Étoiles<br>12 rue des Lilas, Paris</p><a href="https://maps.google.com/?q=Paris" target="_blank" rel="noreferrer">Voir l'itinéraire ↗</a></article>
        <article><span class="feature-badge optional">Option</span><div class="info-icon">♡</div><h3>Tenue de fête</h3><p>Une tenue élégante et festive, dans laquelle vous vous sentez beaux et bien.</p><span>CHIC & ROMANTIQUE</span></article>
        <article><span class="feature-badge optional">Option</span><div class="info-icon">✉</div><h3>Votre réponse</h3><p>Merci de nous confirmer votre présence avant le 15 mai 2027.</p><a href="#rsvp">Répondre ici ↓</a></article>
      </div>
    </section>

    <section id="rsvp" class="rsvp-section">
      <span class="feature-badge optional">Option</span>
      <div class="rsvp-flower">✿</div>
      <p class="eyebrow">ON COMPTE SUR VOUS !</p>
      <p class="script">Un petit mot de votre part</p>
      <h2>Serez-vous <em>avec nous ?</em></h2>
      <p class="rsvp-intro">Dites-nous si vous serez présents pour célébrer cette journée à nos côtés.</p>
      <form v-if="!submitted" @submit.prevent="submitRsvp">
        <label>Votre nom et prénom<input v-model="guestName" required placeholder="Comment vous appelez-vous ?"></label>
        <label>Votre adresse e-mail<input v-model="guestEmail" type="email" required placeholder="vous@exemple.fr"></label>
        <fieldset><legend>Votre réponse</legend><label class="choice"><input v-model="attendance" type="radio" value="oui"> Avec joie, je serai là !</label><label class="choice"><input v-model="attendance" type="radio" value="non"> Je ne pourrai malheureusement pas venir</label></fieldset>
        <div class="guest-picker">
          <span class="field-label">Nombre de personnes</span>
          <button class="guest-trigger" type="button" :aria-expanded="guestCountOpen" aria-haspopup="listbox" @click="guestCountOpen = !guestCountOpen">
            <span>{{ guestCount }} {{ guestCount === '1' ? 'personne' : 'personnes' }}</span><span class="guest-chevron">⌄</span>
          </button>
          <div v-if="guestCountOpen" class="guest-options" role="listbox" aria-label="Nombre de personnes">
            <button v-for="n in 4" :key="n" type="button" role="option" :aria-selected="guestCount === String(n)" :class="{ selected: guestCount === String(n) }" @click="guestCount = String(n); guestCountOpen = false">
              <span>{{ n }} {{ n === 1 ? 'personne' : 'personnes' }}</span><span class="guest-check">{{ guestCount === String(n) ? '✓' : '' }}</span>
            </button>
          </div>
        </div>
        <label>Un petit mot pour nous<textarea v-model="message" rows="3" placeholder="Un mot doux, une pensée…"></textarea></label>
        <button type="submit">Envoyer ma réponse <span>♡</span></button>
      </form>
      <div v-else class="thanks"><span>♡</span><h3>Merci {{ guestName }} !</h3><p>Votre réponse a bien été prise en compte dans cette démonstration.</p></div>
      <small class="form-note">Démonstration : les réponses ne sont pas encore enregistrées ni envoyées.</small>
    </section>

    <footer><div class="footer-monogram">S <span>&</span> D</div><p class="script">Nous avons hâte de vous retrouver</p><p>12 JUIN 2027 · PARIS</p><small>AVEC AMOUR · INVITATION IMAGINÉE PAR C BY C</small></footer>
  </main>
</template>
