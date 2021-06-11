<template>
  <section class="hero is-dark is-fullheight">
    <!-- Hero head: will stick at the top -->
    <div class="hero-head">
      <nav class="navbar">
        <div class="container">
          <div class="navbar-brand">
            <a class="navbar-item">
              <figure class="image is-64x64">
                <img
                    src="./assets/logo.png"
                    alt="Logo"
                    style="max-height: 100%"
                />
              </figure>
            </a>
          </div>
        </div>
      </nav>
    </div>

    <!-- Hero content: will be in the middle -->
    <div class="hero-body">
      <div class="container has-text-centered">
        <h1 class="title">Pomodoro APP</h1>
        <Timer/>
      </div>
    </div>

    <!-- Hero footer: will stick at the bottom -->
    <div class="hero-foot">
      <nav class="tabs">
        <div class="container">
          <ul>
            <li class="is-active">
              <a
                  target="_blank"
                  href="https://github.com/joabsonlg/Landing-Page"
              >Code</a
              >
            </li>
            <li class="is-active">
              <a target="_blank" href="https://github.com/joabsonlg/Landing-Page">Vuetify Landing Page</a>
            </li>
            <li class="is-active">
              <a target="_blank" href="https://v3.vuejs.org/">VUE</a>
            </li>
            <li class="is-active">
              <a target="_blank" href="https://vitejs.dev/">Vite</a>
            </li>
            <li class="is-active">
              <a target="_blank" href="https://bulma.io/">Bulma</a>
            </li>
          </ul>
        </div>
      </nav>
    </div>
  </section>
</template>

<script setup>
import Timer from './components/Timer.vue'

// This starter template is using Vue 3 experimental <script setup> SFCs
// Check out https://github.com/vuejs/rfcs/blob/script-setup-2/active-rfcs/0000-script-setup.md
</script>
