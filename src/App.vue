<script setup>
import { RouterLink, RouterView } from 'vue-router'

</script>

<template>
  <header class="site-header">
    <div class="wrapper">
      <div class="brand">GL Quiz</div>

      <nav>
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/quiz" class="play">Quiz</RouterLink>
        <RouterLink to="/about">About</RouterLink>
      </nav>
    </div>
  </header>

  <RouterView />
</template>

<style scoped>
.site-header {
  width: 100%;
  padding: 0.6rem 0;
  position: sticky;
  top: 0;
  z-index: 40;
  backdrop-filter: blur(6px);
  background: linear-gradient(180deg, rgba(255,255,255,0.01), transparent);
}
.wrapper {
  max-width: 1200px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: clamp(0.5rem, 2vw, 1rem);
  padding: 0 1rem;
}
.brand {
  font-weight: 800;
  letter-spacing: -0.02em;
  color: var(--color-heading);
  font-size: clamp(0.95rem, 1.2vw, 1.05rem);
}
nav {
  display: flex;
  gap: clamp(0.25rem, 1vw, 0.75rem);
  align-items: center;
  overflow-x: auto;
  -webkit-overflow-scrolling: touch;
}
nav::-webkit-scrollbar { display: none; }
nav a {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.45rem 0.65rem;
  border-radius: 999px;
  color: var(--color-text);
  background: transparent;
  border: 1px solid transparent;
  font-size: 0.9rem;
  transition: transform .14s ease, box-shadow .14s ease;
}
nav a:focus-visible { outline: none; box-shadow: 0 6px 24px rgba(3,102,214,0.09); }
nav a:hover { transform: translateY(-2px); box-shadow: 0 8px 28px rgba(2,6,23,0.06); }
nav a.play {
  background: linear-gradient(90deg,var(--accent-1),var(--accent-2));
  color: white;
  padding: 0.55rem 0.9rem;
  font-weight: 700;
}
@media (max-width: 520px) {
  .brand { font-size: 0.95rem }
  nav a { padding: 0.45rem 0.6rem; font-size: 0.85rem }
}
</style>