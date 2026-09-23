# 🛠️ Plantilla Maestra PC1 - PARTE 2: Presentación (Vue UI)

**Instrucciones de uso:** Estos son los componentes visuales. Recuerda la regla de oro: Los archivos físicos se llaman `.component.vue`, pero en el HTML se usan con guiones (ej. `<item-card>`).

## 5. El Componente Tarjeta (El Item Individual)

**Ruta exacta:** `src/universities/components/university-card.component.vue`
*(Si es noticias: `src/news/components/article-card.component.vue`)*

```html
<script setup>
/**
 * @summary Component to display a single item card.
 * @author [Tu Código] - [Tu Nombre y Apellido]
 */
defineProps({
  // ⚠️ CAMBIAR AQUÍ: El nombre del prop según tu entidad (ej. article, coffee, university)
  item: {
    type: Object,
    required: true
  }
});
</script>

<template>
  <!-- ⚠️ CAMBIAR AQUÍ: El diseño interno de la tarjeta según lo que pida la rúbrica -->
  <pv-card class="m-3" style="width: 25rem; overflow: hidden">
    <template #header>
      <!-- Ejemplo de imagen. Ajustar item.urlToLogo a la propiedad real de tu Entidad -->
      <img :src="item.urlToLogo" alt="Logo" class="w-full h-10rem object-contain p-3" />
    </template>
    
    <!-- ⚠️ CAMBIAR AQUÍ: Las propiedades (ej. item.title, item.author) -->
    <template #title>{{ item.name }}</template>
    <template #subtitle>{{ item.country }}</template>
    
    <template #content>
      <p class="font-bold mb-1">Domains:</p>
      <ul>
        <!-- Ejemplo de un arreglo interno (v-for) -->
        <li v-for="domain in item.domains" :key="domain">{{ domain }}</li>
      </ul>
      
      <!-- ⚠️ ALERTA RÚBRICA: Si piden links, deben tener target="_blank" -->
      <a v-if="item.webPages && item.webPages.length" :href="item.webPages[0]" target="_blank" rel="noopener noreferrer">
        <pv-button label="Visit Website" icon="pi pi-external-link" size="small" />
      </a>
    </template>
  </pv-card>
</template>
```

## 6. El Componente Lista (El Contenedor)

**Ruta exacta:** `src/universities/components/university-list.component.vue`
*(Si es noticias: `src/news/components/article-list.component.vue`)*

```html
<script setup>
// ⚠️ CAMBIAR AQUÍ: Importa tu componente tarjeta correcto
import UniversityCard from './university-card.component.vue';

/**
 * @summary Component to display a list of item cards.
 * @author [Tu Código] - [Tu Nombre y Apellido]
 */
defineProps({
  // ⚠️ CAMBIAR AQUÍ: El arreglo que recibes (ej. articles, universities)
  items: {
    type: Array,
    required: true
  }
});
</script>

<template>
  <div class="flex flex-wrap justify-content-center align-items-stretch gap-4">
    <!-- ⚠️ CAMBIAR AQUÍ: El nombre de la etiqueta debe coincidir con el nombre de tu archivo sin el .component.vue -->
    <university-card 
      v-for="item in items" 
      :key="item.name" 
      :item="item" 
    />
  </div>
</template>
```

## 7. El Bounded Context `public` (Elementos Generales)

El profesor pide que elementos como el Toolbar y el Footer vayan en un dominio llamado `public`.

### 7.1 El Toolbar (Header)
**Ruta exacta:** `src/public/components/toolbar.component.vue`

```html
<script setup>
import { useI18n } from 'vue-i18n';

/**
 * @summary Main toolbar with brand and language switcher.
 * @author [Tu Código] - [Tu Nombre y Apellido]
 */
const { locale, availableLocales } = useI18n();
</script>

<template>
  <pv-toolbar class="bg-primary text-white p-3 border-noround">
    <template #start>
      <!-- ⚠️ CAMBIAR AQUÍ: El nombre de la App según el examen -->
      <h2 class="m-0">KnowMyUni / AppName</h2>
    </template>
    <template #end>
      <!-- ⚠️ ALERTA RÚBRICA: Botones para cambiar de idioma EN | ES -->
      <pv-select-button v-model="locale" :options="availableLocales" aria-labelledby="basic" />
    </template>
  </pv-toolbar>
</template>
```

### 7.2 El Footer
**Ruta exacta:** `src/public/components/footer.component.vue`

```html
<script setup>
/**
 * @summary Footer component with author attribution.
 * @author [Tu Código] - [Tu Nombre y Apellido]
 */
</script>

<template>
  <footer class="bg-primary text-white text-center p-3 mt-5">
    <!-- ⚠️ CAMBIAR AQUÍ: Modifica el copyright según el nombre de la app del examen -->
    <p class="m-1">Copyright &copy; 2026 AppName. All rights reserved.</p>
    <!-- ⚠️ CAMBIAR AQUÍ: Tus datos reales -->
    <p class="m-1">Developed by [Tu Código] - [Tu Nombre y Apellido]</p>
  </footer>
</template>
```

## 8. El Archivo Raíz (App.vue)

**Ruta exacta:** `src/App.vue`
*(Nota: Aquí unes todo. Importas la API, pides los datos y los mandas a la lista).*

```html
<script setup>
import { ref, onMounted } from 'vue';

// ⚠️ CAMBIAR AQUÍ: Importa tus componentes y servicios correctos
import ToolbarContent from './public/components/toolbar.component.vue';
import FooterContent from './public/components/footer.component.vue';
import UniversityList from './universities/components/university-list.component.vue';
import { UniversitiesApiService } from './universities/services/universities-api.service.js';

// ⚠️ CAMBIAR AQUÍ: Variable para guardar los datos (ej. articles)
const itemsList = ref([]);

onMounted(async () => {
  // Llama a tu API Service
  itemsList.value = await UniversitiesApiService.getUniversities();
});
</script>

<template>
  <div class="flex flex-column min-h-screen">
    <!-- Header -->
    <toolbar-content />
    
    <!-- Contenido Principal -->
    <main class="flex-grow-1 p-4">
      <!-- ⚠️ CAMBIAR AQUÍ: Usa i18n para el título si quieres, o ponlo fijo -->
      <h1 class="text-center mb-4">{{ $t('home.title') }}</h1>
      
      <!-- Lista de tarjetas -->
      <university-list :items="itemsList" />
    </main>
    
    <!-- Footer -->
    <footer-content />
  </div>
</template>

<style>
/* Estilos globales básicos */
body {
  margin: 0;
  padding: 0;
  font-family: var(--font-family); /* Tipografía de PrimeVue */
  background-color: var(--surface-ground);
}
</style>
```