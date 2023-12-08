<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <header>
    <div class="wrapper">
      <nav>
        <RouterLink to="/">Le jeu de la vie</RouterLink> |
        <RouterLink to="/about">A propos</RouterLink>
      </nav>
    </div>
  </header>

  <RouterView />
</template>
