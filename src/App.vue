<script setup lang="ts">
import HeaderItem from "./components/HeaderItem.vue";
import TheSelections from "./components/TheSelections.vue";
import Selection from "./components/Selection.vue";
</script>

<template>
  <header>
    <Header-Item />
  </header>

  <main>
    <div class="wrapper">
      <The-Selections />
    </div>
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
  background: #fd7;
  padding: 2rem;
}
.wrapper {
  max-width: 800px;
  margin: auto;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
