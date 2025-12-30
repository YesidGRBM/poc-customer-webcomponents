<script setup lang="ts">
import { ref } from 'vue'


const loading = ref(false)
const error = ref<string|undefined>(undefined)

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

const addStyleSheet = () => {
  const sheet = document.createElement('link')
  sheet.type = 'text/css'
  sheet.rel = 'stylesheet'
  sheet.href = 'https://pub-d76b263501ba4f559a765fe05f6e4b2b.r2.dev/css/style.css'
  document.head.appendChild(sheet)
}
</script>

<template>
  <Teleport to="body">
    <div v-if="loading" class="over">
      <div class="modal">
        Cargando el script.
      </div>
    </div>
  </Teleport>
  <button @click="addStyleSheet">Agregar hoja de estilos</button>
  <ul>
    <li v-for="webcomponent in webcomponents" :key="webcomponent">
      <button @click="loader(webcomponent)">
        {{ webcomponent }}
      </button>
    </li>
  </ul>
</template>
