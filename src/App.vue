<script setup lang="ts">
import { ref } from 'vue'

const loading = ref(false)
const error = ref<string | undefined>(undefined)

const webcomponents = ref(['lit', 'react', 'stencil', 'svelte', 'vue'])

const loader = (path: string) => {
  loading.value = true;

  const script = document.createElement('script')

  script.src = `https://pub-d76b263501ba4f559a765fe05f6e4b2b.r2.dev/v1/${path}/main.js`

  script.addEventListener('load', () => {
    loading.value = false;
  })

  script.addEventListener('error', (e) => {
    error.value = e.error
  })

  document.head.appendChild(script);

  const component = document.createElement(`syrbm-terms-${path}`)

  document.body.appendChild(component);
}

</script>

<template>
  <Teleport to="body">
    <dialog v-if="loading" open>
      <article>
        <p>
          Cargando el script.
        </p>
      </article>
    </dialog>
  </Teleport>
  <header>
    <button v-for="webcomponent in webcomponents" :key="webcomponent" class="outline" @click="loader(webcomponent)">
      {{ webcomponent }}
    </button>
  </header>
</template>

<style scoped>
  header {
    display: flex;
    width: fit-content;
    margin: auto;
    gap: 1rem;
  }

  button {
    flex: 1;
  }
</style>
