<script setup>
import { useRouter } from 'vue-router'
const router = useRouter()
function start() {
  router.push('/quiz')
}
</script>

<template>
  <main class="home-hero">
    <div class="hero-card container">
      <h1>Welcome to <span class="brand">GL Quiz</span></h1>
      <p class="lead">10 quick Scrum questions — fast, fun and responsive. Ready to test your knowledge?</p>

      <div class="actions">
        <button class="btn cta" @click="start" aria-label="Let's play">Let's play</button>
      </div>

      <p class="small">Responsive • Instant feedback • No signup</p>
    </div>
  </main>
</template>

<style scoped>
.home-hero {
  min-height: calc(100vh - 72px);
  display: grid;
  place-items: center;
  padding: clamp(1rem, 3vw, 2.5rem) 1rem;
  background: radial-gradient(800px 400px at 10% 10%, rgba(124,58,237,0.04), transparent),
              radial-gradient(700px 300px at 90% 90%, rgba(6,182,212,0.03), transparent);
}
.hero-card {
  width: 100%;
  max-width: 980px;
  text-align: center;
  background: linear-gradient(135deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
  border-radius: 16px;
  padding: clamp(1.25rem, 3vw, 3rem);
  box-shadow: 0 18px 60px rgba(2,6,23,0.12);
  border: 1px solid rgba(255,255,255,0.04);
  backdrop-filter: blur(6px);
}
.hero-card h1 { margin: 0 0 0.5rem 0; font-size: clamp(1.5rem, 3vw, 2.1rem); line-height:1.05 }
.brand { background: linear-gradient(90deg,var(--accent-1),var(--accent-2)); -webkit-background-clip: text; background-clip: text; color: transparent; font-weight: 900; }
.lead { margin-bottom: 1.25rem; color: var(--color-text); opacity: 0.9; font-size: clamp(0.95rem, 1.4vw, 1.05rem); }
.actions { margin-bottom: 0.75rem }
.actions .cta {
  font-size: clamp(0.98rem, 1.6vw, 1.08rem);
  padding: clamp(0.7rem, 1.6vw, 1rem) clamp(1.4rem, 3vw, 2.2rem);
  border-radius: 999px;
  border: 0;
  background: linear-gradient(90deg,var(--accent-1),var(--accent-2));
  color: white;
  box-shadow: 0 12px 40px rgba(7,89,117,0.12);
  cursor: pointer;
  transition: transform .14s cubic-bezier(.2,.9,.3,1), box-shadow .14s ease;
}
.actions .cta:hover { transform: translateY(-4px); box-shadow: 0 22px 60px rgba(7,89,117,0.14); }
.actions .cta:active { transform: translateY(0) scale(.998); }
.small { margin-top: 1rem; color: rgba(255,255,255,0.65); opacity: 0.85 }
@media (prefers-color-scheme: dark) {
  .hero-card { background: linear-gradient(135deg, rgba(12,12,14,0.6), rgba(18,18,20,0.45)); border: 1px solid rgba(255,255,255,0.02); }
  .small, .lead { color: var(--color-text); opacity: 0.76; }
  .hero-card h1 { color: var(--color-heading); }
}
@media (max-width: 520px) {
  .hero-card { padding: clamp(1rem, 4vw, 1.5rem); border-radius: 12px; }
  .hero-card h1 { font-size: 1.3rem; }
  .actions .cta { width: 100%; padding: 0.85rem; }
}
</style>