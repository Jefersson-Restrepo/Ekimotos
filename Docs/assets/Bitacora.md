# 📝 Bitácora de Desarrollo - EKI-MOTOS Taller

Este archivo registra el avance, acuerdos y transformaciones del proyecto web de EKI-MOTOS.

## 📌 Proyecto: Sitio Web de EKI-MOTOS Taller
* **Estado:** Planificación y Análisis.
* **Cliente:** Taller de Motos EKI-MOTOS.
* **Desarrollador:** (Tu nombre) con asistencia de IA.

---

## 📅 Registro de Cambios y Planificación

### [2026-10-29] - Fase 1: Arquitectura, Roles y Modelado UML
* **Análisis de Requerimientos:**
  * Identificación del modelo de negocio de una microempresa con 15 años de experiencia.
  * Definición de roles críticos: 
    * **Gerente/Dueño (Mecánico Especialista):** Administración del taller y reparaciones de alta complejidad (motores, electricidad, cilindros, ramales).
    * **Trabajador (Mecánico General):** Mantenimiento regular (aceites, bandas, kits de arrastre, barras, bombillería).
    * **Contador:** Encargado de finanzas y balances (sin rol de taller directo).
* **Entregables de hoy:**
  * Estructuración del archivo de bitácora (`bitacora.md`).
  * Análisis conceptual de roles para el taller.
  * Creación del Diagrama de Clases UML para mapear la lógica de negocio antes de la base de datos y de programar en VS Code.

### 🎯 Próximos Pasos:
* [ ] Crear estructura de carpetas en Visual Studio Code (Assets, JS, CSS, Docs).
* [ ] Maquetación del HTML5 base (`index.html` y `servicios.html`).
* [ ] Inicialización del repositorio Git y publicación en GitHub Pages para pruebas del cliente.


-----------------------------------------------------------------------------------


### [2026-10-29] - Fase 2: Desarrollo del Maquetado e Inicio Semántico
* **Estructura HTML5**:
  - Implementación de marcado semántico completo (`<header>`, `<nav>`, `<main>`, `<section>`, `<figure>`), evitando el abuso de `<div>`.
  - Configuración del árbol de dependencias local conectando estilos y recursos adecuadamente según el espacio de trabajo local (`Docs/...`).
* **Estilos CSS**:
  - Implementación de Flexbox y CSS Grid para la distribución de portada de alto impacto visual con diseño 2 columnas para resolución de escritorio.
  - Maquetación Mobile-First Responsiva con menú interactivo hamburguesa de bajo consumo para carga móvil acelerada.
  - Paleta premium activada: base de negro carbón (`#121212`) con relieves naranja (`#FF5A00`).
* **Interactividad JS**:
  - Conexión de menú responsivo nativo en el script del proyecto.

  ---------------------------------------------------------------------------

  # 🛠️ Explicación Técnica: ¿Por qué la imagen de fondo debe ir en CSS y cómo funcionan las rutas (`../`)?

Este documento detalla las decisiones de arquitectura de frontend y la resolución de problemas de rutas aplicadas en la página de inicio (`index.html` y `Stylo.css`) de **EKI-MOTOS**, orientadas a un desarrollo semántico, optimizado y de nivel profesional (Nivel 3).

---

## 1. ¿Por qué la imagen de fondo de la portada debe ir en el CSS y no en el HTML?

En el desarrollo web profesional, existe una regla de oro: **separar el contenido (HTML) de la presentación y el diseño (CSS)**.

Si hubiéramos dejado la imagen de la moto en el HTML usando una etiqueta `<img>` o `<picture>`, habríamos tenido los siguientes problemas:

* **Saturación y "ruido" en el marcado (HTML):** Hubiéramos necesitado añadir etiquetas `<div>` o `<figure>` adicionales para forzar a la imagen a estirarse, lo que va en contra de nuestra meta de evitar el sobreuso de contenedores innecesarios (*divitis*).
* **Problemas graves de accesibilidad (A11y):** Las imágenes de fondo son **decorativas o de ambientación**, no contenido crítico para el usuario. Si la ponemos en el HTML, los lectores de pantalla para personas con discapacidad visual intentarían leerla o describirla, lo cual entorpece la navegación. Al ponerla en CSS, queda claro que es un elemento de diseño.
* **Falta de control sobre el texto (Legibilidad):** En CSS, podemos usar la propiedad `background-image` para superponer un degradado oscuro (`linear-gradient`) sobre la imagen en una sola línea de código. Esto garantiza que el texto "EL TALLER DE CONFIANZA..." sea 100% legible. Lograr esto mismo con una imagen en HTML requiere un posicionamiento absoluto complejo, capas de `z-index` y contenedores adicionales que cargan la página.
* **Adaptabilidad (Responsive) simplificada:** Con CSS, propiedades como `background-size: cover;` y `background-position: center right;` permiten que el navegador recorte y adapte la imagen de fondo automáticamente en celulares, tablets o pantallas gigantes, sin que la moto se aplaste o se deforme.

---

## 2. La lógica de la ruta relativa (`../`) en CSS: ¿Por qué fallaba antes?

El error por el cual la pantalla se veía completamente negra se debió a un fallo en la **ruta relativa** del archivo. 

### El escenario del error:
La estructura de carpetas de nuestro proyecto en Visual Studio Code es la siguiente:

```text
Eki_motos/
│
├── index.html                  <-- El usuario lo abre desde aquí
│
└── Docs/
    └── assets/
        ├── css/
        │   └── Stylo.css       <-- Aquí es donde se ejecuta el CSS
        │
        └── Imagenes/
            └── Motos/
                └── Eki_motos.jpg  <-- El recurso que necesitamos
```

### ¿Por qué fallaba `Docs/assets/Imagenes/...`?
Cuando el navegador lee el archivo `Stylo.css` e intenta cargar la imagen, calcula la ruta **desde el lugar donde está el archivo CSS** (`Docs/assets/css/`), NO desde la raíz del proyecto ni desde el `index.html`.

Si escribimos:
`url('Docs/assets/Imagenes/Motos/Eki_motos.jpg')`  
El navegador busca la imagen en:  
❌ `Docs/assets/css/Docs/assets/Imagenes/Motos/Eki_motos.jpg` (Ruta inexistente).

### La solución: El operador de subida de nivel (`../`)
Para solucionarlo, le indicamos al navegador que debe "salir" de las carpetas donde se encuentra para buscar el recurso en la carpeta hermana:

1. **`..`** : Le dice al navegador "sal de la carpeta actual" (`css/`). Ahora estamos en la carpeta `assets/`.
2. **`/`** : Entra a la siguiente ruta especificada.
3. **`Imagenes/Motos/Eki_motos.jpg`** : Entra a la carpeta de imágenes, luego a motos y selecciona el archivo.

Por lo tanto, la ruta relativa correcta desde el CSS es:  
`url('../Imagenes/Motos/Eki_motos.jpg')`

---

## 3. Ventajas obtenidas en el Rendimiento y la Mantenibilidad

1. **Código Liviano:** El HTML es sumamente limpio y rápido de leer para los buscadores (SEO).
2. **Control Centralizado:** Si el taller decide cambiar la moto KTM por otra de fondo en el futuro, no hay que tocar el HTML de ninguna página; se cambia en una sola línea del CSS y se actualizará en todo el sitio web.
3. **Mantenimiento del Contraste:** Gracias al degradado, no dependemos de que la foto que suba el cliente sea perfecta; el CSS siempre garantizará que el texto sea visible sin importar qué haya detrás.


----------------------------------------------------------------------------



### [2026-10-29] - Fase 2.1: Optimización de Assets y Despliegue CI/CD
* **Optimización visual**:
  - Corrección de renderizado del logo (eliminación de fondo blanco JPG a PNG transparente).
  - Aplicación de `filter: drop-shadow` en CSS para integrar el logo orgánicamente a la barra de navegación oscura.
* **Control de Versiones y Despliegue**:
  - Inicialización del repositorio Git local y primer commit semántico.
  - Conexión con repositorio remoto en GitHub.
  - Despliegue continuo activado mediante GitHub Pages para revisión de mockups interactivos por parte del cliente (Entorno de Producción inicial).