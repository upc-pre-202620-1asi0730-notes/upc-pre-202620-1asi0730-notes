
**Instrucciones:** Copia y pega estos archivos respetando estrictamente las rutas indicadas. Busca los comentarios `// CAMBIAR AQUÍ:` para adaptar la plantilla al problema específico que te ponga el profesor.

---

## 1. El Servicio Compartido (Shared Kernel)
**Ruta exacta:** `src/shared/services/logo-api.service.js`
*(Nota: El profesor pide Clearbit, pero podría pedir Logo.dev. Esta clase sirve para cualquier API de logos)*

```javascript
/**
 * @summary Service to generate logo URLs based on domains.
 * @author [Tu Código] - [Tu Nombre y Apellido]
 */
export class LogoApiService {
    static getUrlToLogo(domain) {
        // CAMBIAR AQUÍ: Si el profesor pide Logo.dev o falla el dominio, cambias esta lógica.
        if (!domain) return 'https://via.placeholder.com/150';
        
        // CAMBIAR AQUÍ: URL base del servicio de logos (Clearbit en este caso)
        return `https://logo.clearbit.com/${domain}`;
    }
}
```

---

## 2. La Entidad de Dominio (Domain Model)
**Ruta exacta:** `src/universities/model/university.entity.js`
*(Nota: Si en el examen te piden "Cafés" o "Películas", cambia el nombre del archivo y la clase, por ejemplo: `coffee.entity.js`)*

```javascript
/**
 * @summary Domain entity representing a University.
 * @author [Tu Código] - [Tu Nombre y Apellido]
 */
export class University {
    //  CAMBIAR AQUÍ: En los parámetros del constructor van los atributos que TE PIDA LA RÚBRICA.
    // Ojo: Usa camelCase aquí (ej. alphaTwoCode), nunca snake_case (alpha_two_code).
    constructor({ name, country, alphaTwoCode, domains, webPages, urlToLogo }) {
        this.name = name;
        this.country = country;
        this.alphaTwoCode = alphaTwoCode;
        this.domains = domains || [];
        this.webPages = webPages || [];
        
        // Atributo extra calculado (el logo)
        this.urlToLogo = urlToLogo;
    }
}
```

---

## 3. El Ensamblador (Assembler / Anti-Corruption Layer)
**Ruta exacta:** `src/universities/services/university-assembler.service.js`
*(Nota: Aquí es donde traduces el JSON en inglés raro que manda la API, a tu Entidad pura)*

```javascript
import { University } from '../model/university.entity.js';
import { LogoApiService } from '../../shared/services/logo-api.service.js';

/**
 * @summary Assembler to map API JSON resources to Domain Entities.
 * @author [Tu Código] - [Tu Nombre y Apellido]
 */
export class UniversityAssembler {
    static toEntity(resource) {
        //  CAMBIAR AQUÍ: Lógica especial para calcular datos (como extraer el primer dominio para el logo)
        const firstDomain = resource.domains && resource.domains.length > 0 ? resource.domains[0] : null;
        const logoUrl = LogoApiService.getUrlToLogo(firstDomain);

        //  CAMBIAR AQUÍ: Mapeo exacto. 
        // A la izquierda: El nombre de tu variable en el constructor de la Entidad.
        // A la derecha: resource.nombre_del_campo_exacto_del_JSON_de_la_API
        return new University({
            name: resource.name,
            country: resource.country,
            alphaTwoCode: resource.alpha_two_code, // Traducción de snake_case a camelCase
            domains: resource.domains,
            webPages: resource.web_pages,
            urlToLogo: logoUrl
        });
    }

    static toEntities(resources) {
        // Este método nunca cambia, siempre es igual para mapear arreglos.
        return resources.map(resource => this.toEntity(resource));
    }
}
```

---

## 4. El Servicio API (Infrastructure / Axios)
**Ruta exacta:** `src/universities/services/universities-api.service.js`
*(Nota: El orquestador que hace la llamada HTTP)*

```javascript
import axios from 'axios';
import { UniversityAssembler } from './university-assembler.service.js';

/**
 * @summary API service to fetch data from the external provider.
 * @author [Tu Código] - [Tu Nombre y Apellido]
 */
export class UniversitiesApiService {
    
    // CAMBIAR AQUÍ: El nombre del método según lo que busques (ej. getCoffees)
    static async getUniversities() {
        try {
            // CAMBIAR AQUÍ: Las URLs exactas que te dé el profesor en el PDF.
            // Si te pide hacer dos llamadas a la vez, usa Promise.all así:
            const [response1, response2] = await Promise.all([
                axios.get('http://universities.hipolabs.com/search?country=spain'),
                axios.get('http://universities.hipolabs.com/search?country=peru')
            ]);

            // CAMBIAR AQUÍ: Consolidar la data. 
            // Cuidado: Algunas APIs devuelven arreglo directo (response.data), 
            // otras lo devuelven dentro de un objeto (response.data.results). ¡Revisa el JSON!
            const combinedData = [...response1.data, ...response2.data];
            
            // Pasar la data cruda por el escudo del Assembler
            return UniversityAssembler.toEntities(combinedData);
            
        } catch (error) {
            console.error("Error fetching data from API:", error);
            // CAMBIAR AQUÍ: Retornar un arreglo vacío para que la vista no se rompa
            return [];
        }
    }
}
```