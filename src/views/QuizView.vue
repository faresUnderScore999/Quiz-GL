<script setup>
import { ref, computed, watch } from 'vue'
import questions from '../assets/questions.json'

const total = questions.length
const current = ref(0)
const selected = ref(null)
const score = ref(0)
const locked = ref(false)
const finished = ref(false)
const advanceTimer = ref(null)
const labels = ['A', 'B', 'C', 'D']

const q = computed(() => questions[current.value])
const percentage = computed(() => Math.round((score.value / total) * 100))
const progress = computed(() => Math.round(((current.value) / total) * 100))

function selectOption(idx) {
  if (locked.value || finished.value) return
  selected.value = idx
  locked.value = true

  if (idx === q.value.correctIndex) score.value++

  // auto-advance after short delay
  advanceTimer.value = setTimeout(() => {
    if (current.value + 1 < total) {
      current.value++
      selected.value = null
      locked.value = false
    } else {
      finished.value = true
    }
  }, 900)
}

function skip() {
  if (locked.value || finished.value) return
  if (current.value + 1 < total) {
    current.value++
  } else {
    finished.value = true
  }
}

function restart() {
  clearTimeout(advanceTimer.value)
  current.value = 0
  selected.value = null
  score.value = 0
  locked.value = false
  finished.value = false
}

function optionClass(idx) {
  if (!locked.value) return ''
  if (idx === q.value.correctIndex) return 'correct'
  if (selected.value === idx && idx !== q.value.correctIndex) return 'wrong'
  return 'dim'
}

const resultMessage = computed(() => {
  if (percentage.value === 100) return 'Parfait — 10/10 ! 🎉'
  if (percentage.value >= 70) return 'Bon travail — vous maitrisez bien Scrum ✅'
  if (percentage.value >= 40) return 'Pas mal — un peu de révision 🔁'
  return 'À revoir — bonne chance la prochaine fois 🤓'
})

// clear timer on route change / component unmount
watch(finished, (v) => { if (v) clearTimeout(advanceTimer.value) })
</script>

<template>
  <section class="quiz-page">
    <header class="quiz-header">
      <div>
        <h1>Quiz Scrum — Testez vos connaissances</h1>
        <p class="sub">10 questions rapides • Responsive • Feedback instantané</p>
      </div>
      <div class="score">
        <div class="score-value">{{ score }} / {{ total }}</div>
        <div class="small">Points</div>
      </div>
    </header>

    <div class="progress">
      <div class="progress-bar" :style="{ width: progress + '%' }"></div>
    </div>

    <div class="card" v-if="!finished">
      <div class="meta">
        <div class="q-index">Question {{ current + 1 }} / {{ total }}</div>
      </div>

      <div class="question">{{ q.question }}</div>

      <div class="options">
        <button
          v-for="(opt, idx) in q.options"
          :key="idx"
          class="option"
          :class="optionClass(idx)"
          @click="selectOption(idx)"
          :disabled="locked"
        >
          <span class="label">{{ labels[idx] }}</span>
          <span class="text">{{ opt }}</span>
          <span class="tick" v-if="locked && idx === q.correctIndex">✔</span>
        </button>
      </div>

      <div class="controls">
        <button class="btn ghost" @click="skip" :disabled="locked">Passer</button>
      </div>
    </div>

    <div class="card result" v-else>
      <h2>Résultat</h2>
      <p class="big">{{ score }} / {{ total }} — {{ percentage }}%</p>
      <p class="message">{{ resultMessage }}</p>

      <div class="result-actions">
        <button class="btn" @click="restart">Rejouer</button>
        <router-link to="/" class="btn ghost">Retour</router-link>
      </div>

      <details class="answers">
        <summary>Voir les réponses</summary>
        <ol>
          <li v-for="(item, i) in questions" :key="i">
            <strong>{{ i + 1 }}.</strong>
            {{ item.question }} —
            <em>{{ item.options[item.correctIndex] }}</em>
          </li>
        </ol>
      </details>
    </div>
  </section>
</template>

<style scoped>
.quiz-page {
  width: min(980px, 94%);
  margin: clamp(1rem, 3vw, 2.25rem) auto;
  padding: clamp(0.6rem, 2vw, 1.1rem);
  display: flex;
  flex-direction: column;
  gap: clamp(0.75rem, 2vw, 1.25rem);
}

.quiz-header {
  display: flex;
  gap: clamp(0.5rem, 2vw, 1rem);
  align-items: center;
  justify-content: space-between;
  margin-bottom: 0.75rem;
  flex-wrap: wrap;
}
.quiz-header > div { min-width: 0 }
.quiz-header h1 {
  margin: 0 0 0.25rem 0;
  font-size: clamp(1.1rem, 2vw, 1.6rem);
  line-height: 1.15;
}
.quiz-header .sub {
  margin: 0;
  color: var(--color-text);
  opacity: 0.85;
  font-size: clamp(0.85rem, 1.2vw, 1rem);
}
.score {
  text-align: right;
}
.score-value {
  font-weight: 800;
  font-size: clamp(1rem, 2vw, 1.25rem);
}

.progress {
  height: 10px;
  background: linear-gradient(90deg, rgba(255,255,255,0.03), rgba(255,255,255,0.01));
  border-radius: 999px;
  overflow: hidden;
  margin-bottom: 0.75rem;
  transition: box-shadow .18s ease;
}
@media (max-width:520px) {
  .progress { position: sticky; top: 72px; z-index: 30; box-shadow: 0 6px 30px rgba(2,6,23,0.12); }
}
.progress-bar {
  height: 100%;
  background: linear-gradient(90deg,var(--accent-1),var(--accent-2));
  width: 0%;
  transition: width 420ms cubic-bezier(.2,.9,.2,1);
}

.card {
  background: linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
  border: 1px solid var(--color-border);
  border-radius: 14px;
  padding: clamp(0.75rem, 1.6vw, 1.4rem);
  box-shadow: 0 8px 36px rgba(2,6,23,0.08);
}

.meta { margin-bottom: 0.5rem; color: #9aa3b2; font-size: 0.95rem }
.question {
  font-size: clamp(1rem, 1.8vw, 1.2rem);
  margin: 0.5rem 0 1rem 0;
}
.options {
  display: grid;
  gap: clamp(0.5rem, 1.4vw, 1rem);
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
}
.option {
  display: flex;
  align-items: center;
  gap: 0.9rem;
  padding: clamp(0.8rem, 1.6vw, 1rem);
  border-radius: 12px;
  background: linear-gradient(180deg, var(--color-background-soft), var(--color-background));
  border: 1px solid var(--color-border);
  cursor: pointer;
  transition: transform 160ms ease, box-shadow 160ms ease, background 180ms ease;
  text-align: left;
  position: relative;
  min-height: 64px;
  font-size: clamp(0.95rem, 1.2vw, 1rem);
}
.option:focus-visible { outline: none; box-shadow: 0 6px 30px rgba(3,102,214,0.12); transform: translateY(-2px); }
.option:hover { transform: translateY(-4px); box-shadow: 0 14px 40px rgba(2,6,23,0.08); }
.option .label {
  width: clamp(36px, 4vw, 44px);
  height: clamp(36px, 4vw, 44px);
  display: inline-grid;
  place-items: center;
  border-radius: 10px;
  background: rgba(0,0,0,0.04);
  font-weight: 800;
  font-size: clamp(0.9rem, 1.2vw, 1rem);
  
    color: #ffffff8a;
}
.option .text{
color: #ffffffd1;
}
@keyframes popPulse { 0%{ transform: scale(.98) } 60%{ transform: scale(1.02) } 100%{ transform: scale(1) } }
.option.correct { border-color: #10b981; background: linear-gradient(90deg, rgba(16,185,129,0.06), rgba(16,185,129,0.02)); animation: popPulse .28s cubic-bezier(.2,.9,.2,1); }
.option.wrong { border-color: #ef4444; background: linear-gradient(90deg, rgba(239,68,68,0.04), rgba(239,68,68,0.02)); opacity: 0.98; }
.option.dim { opacity: 0.6; }
.option .tick { position: absolute; right: 12px; top: 12px; color: #10b981; font-weight: 700 }

.controls { margin-top: 0.9rem; display:flex; gap:0.5rem; justify-content:flex-end }
@media (max-width:520px) { .controls { justify-content: stretch } .controls .btn { width: 100%; } }
.btn { border-radius: 10px; padding: 0.6rem 0.9rem; border: 1px solid var(--color-border); background: transparent; cursor: pointer }
.btn:hover { transform: translateY(-2px) }
.btn.ghost { background: transparent }
.btn.primary { background: linear-gradient(90deg,var(--accent-1),var(--accent-2)); color: white; border: 0 }

.result { text-align: center }
.result .big { font-size: clamp(1.4rem, 4vw, 2.3rem); margin: 0.25rem 0 }
.result .message { margin-bottom: 1rem }
.answers { margin-top: 1rem; text-align:left }

@media (min-width: 1024px) {
  .options { gap: 1rem }
  .quiz-header h1 { font-size: 1.8rem }
}

@media (max-width: 520px) {
  .quiz-page { padding: 0.6rem }
  .option { padding: 0.7rem }
}

</style>