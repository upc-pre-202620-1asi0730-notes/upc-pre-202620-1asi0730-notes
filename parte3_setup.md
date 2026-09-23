# 🛠️ Plantilla Maestra PC1 - PARTE 3: Configuración (i18n y Main)

**Instrucciones de uso:** Estos archivos son el motor de tu aplicación. Van en la raíz de la carpeta `src` (o en la carpeta `locales` para los diccionarios). Si copias y pegas esto, tu app arrancará sin errores de configuración.

## 9. Los Diccionarios de Idiomas (i18n)

**Ruta exacta:** `src/locales/en.json` y `src/locales/es.json`
*(Nota: Mantén siempre la misma estructura de llaves en ambos archivos)*

### 9.1 Archivo Inglés (`en.json`)
```json
{
  "home": {
    "title": "Welcome to KnowMyUni"
  },
  "toolbar": {
    "brand": "KnowMyUni"
  },
  "card": {
    "visit": "Visit Website",
    "domains": "Domains"
  },
  "footer": {
    "copyright": "Copyright © 2026 {appName}. All rights reserved.",
    "developedBy": "Developed by {author}"
  }
}
```

### 9.2 Archivo Español (`es.json`)
```json
{
  "home": {
    "title": "Bienvenido a KnowMyUni"
  },
  "toolbar": {
    "brand": "KnowMyUni"
  },
  "card": {
    "visit": "Visitar Sitio Web",
    "domains": "Dominios"
  },
  "footer": {
    "copyright": "Derechos de autor © 2026 {appName}. Todos los derechos reservados.",
    "developedBy": "Desarrollado por {author}"
  }
}
```

## 10. La Configuración de Vue-i18n

**Ruta exacta:** `src/i18n.js`
*(Nota: Este archivo junta tus diccionarios y activa el modo moderno de Vue)*

```javascript
import { createI18n } from 'vue-i18n';

// ⚠️ CAMBIAR AQUÍ: Asegúrate de que las rutas a tus JSON sean correctas
import en from './locales/en.json';
import es from './locales/es.json';

/**
 * @summary Internationalization configuration.
 * @author [Tu Código] - [Tu Nombre y Apellido]
 */
const i18n = createI18n({
    legacy: false, // ⚠️ CRÍTICO: Debe ser false para usar Composition API (<script setup>)
    locale: 'en', // Idioma por defecto que pide la rúbrica
    fallbackLocale: 'en',
    globalInjection: true, // Permite usar $t en los templates directamente
    messages: {
        en: en,
        es: es
    }
});

export default i18n;
```

## 11. El Punto de Entrada (Main.js)

**Ruta exacta:** `src/main.js`
*(Nota: Aquí es donde conectas Vue, PrimeVue, i18n y tus estilos. ¡No olvides ningún import!)*

```javascript
import { createApp } from 'vue';
import App from './App.vue';

// 1. Importar i18n
import i18n from './i18n.js';

// 2. Importar configuración de PrimeVue y Tema (Material)
import PrimeVue from 'primevue/config';
import Material from '@primeuix/themes/material'; // Tema exigido por la rúbrica

// 3. Importar utilidades CSS (Flexbox e Íconos)
import 'primeicons/primeicons.css';
import 'primeflex/primeflex.css';
import './style.css'; // Tus estilos globales

// 4. Importar Componentes de PrimeVue que vayas a usar
import { Button, SelectButton, Card, Toolbar } from 'primevue';

/**
 * @summary Main entry point for the Vue application.
 * @author [Tu Código] - [Tu Nombre y Apellido]
 */
const app = createApp(App);

// ⚠️ CAMBIAR AQUÍ: Agrega tu API Key de PrimeUI si la rúbrica lo exige (opcional dependiendo de la versión de PrimeVue)
// const primeUiLicenseKey = import.meta.env.VITE_PRIME_UI_LICENSE_KEY;

// Conectar plugins
app.use(i18n);
app.use(PrimeVue, { 
    theme: { preset: Material }, 
    ripple: true 
    // license: primeUiLicenseKey 
});

// Registrar componentes globalmente con prefijo 'pv-' (Exigencia de la rúbrica)
app.component('pv-button', Button);
app.component('pv-select-button', SelectButton);
app.component('pv-card', Card);
app.component('pv-toolbar', Toolbar);

// Montar la app
app.mount('#app');
```

## 12. (BONUS) Alias en Vite Config

**Ruta exacta:** `vite.config.js` (En la raíz del proyecto, fuera de `src`)
*(Nota: Para poder importar cosas fácilmente sin usar `../../../`)*

```javascript
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import { fileURLToPath, URL } from 'node:url'

export default defineConfig({
  plugins: [vue()],
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url))
    }
  }
})
```