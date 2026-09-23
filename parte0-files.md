# 📂 MAPA DE CARPETAS - PLANTILLA MAESTRA PC1

**Instrucciones:** Apenas ejecutes el comando de Vite (`npm create vite@latest pc1nrcucode...`) y entres a la carpeta `src/`, debes borrar los archivos de ejemplo (como `HelloWorld.vue`) y crear **exactamente** esta estructura.

> **Regla de Oro de la Rúbrica:** Todo debe estar organizado por **Bounded Contexts** (dominios). Nunca mezcles componentes genéricos con los del negocio.

```text
📦 src
 ┣ 📂 locales                 <-- (Parte 3: Diccionarios i18n)
 ┃ ┣ 📜 en.json
 ┃ ┗ 📜 es.json
 ┃
 ┣ 📂 public                  <-- (Parte 2: Elementos genéricos de la UI)
 ┃ ┗ 📂 components
 ┃   ┣ 📜 footer.component.vue
 ┃   ┗ 📜 toolbar.component.vue
 ┃
 ┣ 📂 shared                  <-- (Parte 1: Servicios de uso común para toda la app)
 ┃ ┗ 📂 services
 ┃   ┗ 📜 logo-api.service.js
 ┃
 ┣ 📂 universities            <-- ⚠️ CAMBIAR AQUÍ: El nombre del dominio del examen (ej. news, coffees, movies)
 ┃ ┣ 📂 components            <-- (Parte 2: Vistas exclusivas de la entidad)
 ┃ ┃ ┣ 📜 university-card.component.vue
 ┃ ┃ ┗ 📜 university-list.component.vue
 ┃ ┃
 ┃ ┣ 📂 model                 <-- (Parte 1: Lógica de negocio pura)
 ┃ ┃ ┗ 📜 university.entity.js
 ┃ ┃
 ┃ ┗ 📂 services              <-- (Parte 1: Infraestructura y Transformación)
 ┃   ┣ 📜 universities-api.service.js
 ┃   ┗ 📜 university-assembler.service.js
 ┃
 ┣ 📜 App.vue                 <-- (Parte 2: El ensamblador visual)
 ┣ 📜 i18n.js                 <-- (Parte 3: Configuración de idiomas)
 ┣ 📜 main.js                 <-- (Parte 3: Punto de entrada, PrimeVue)
 ┗ 📜 style.css               <-- (Estilos y reseteos globales)
