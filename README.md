# EduPlay

**Aprende jugando, supera cada reto.**

EduPlay es una aplicación educativa gamificada para estudiantes de los grados 9°, 10° y 11°. Organiza contenidos de Matemáticas, Física, Lectura crítica y Ciencias Sociales y Ciudadanas por grado. Tiene retos con retroalimentación inmediata, simulacros tipo ICFES cronometrados, puntos, niveles, insignias, tabla de clasificación y panel de progreso.

Proyecto productivo SENATIC · Técnico en Programación de Aplicaciones para Dispositivos Móviles
Centro Acuícola y Agroindustrial de Gaira (SENA) · Institución Educativa Técnica Departamental Pijiño del Carmen, Magdalena · Ficha 3321831

## Funcionalidades del PMV

| Módulo | Qué hace |
| --- | --- |
| Registro e inicio de sesión | Cuenta con nombre, usuario, grado, institución y contraseña. Incluye la autorización de tratamiento de datos (Ley 1581 de 2012). La contraseña se guarda cifrada con SHA-256 y sal. |
| Aprender | 28 temas organizados por grado y área, cada uno con explicación, ejemplo y enlaces a YouTube, TikTok, Khan Academy y Wikipedia. |
| Retos | 5 preguntas por área y nivel (Básico 9°, Medio 10°, Avanzado 11°), con la explicación de cada respuesta al instante. Cada acierto suma 10 puntos. |
| Simulacro ICFES | 20 preguntas (5 por área), 30 minutos, navegación libre y revisión al final. Da un puntaje estimado de 0 a 500. |
| Gamificación | 6 niveles (Aprendiz a Leyenda), 9 insignias y tabla de clasificación. |
| Biblioteca | Base de datos de fotos, videos y enlaces subidos por la comunidad, más recursos seleccionados de la web y redes sociales: cuadernillos ICFES 2026, Colombia Aprende, PhET, GeoGebra y canales de YouTube. |
| Perfil | Progreso por área, insignias, historial de simulacros y cierre de sesión. |

El banco inicial tiene **48 preguntas tipo Saber 11**: 4 por área y grado.

## Estructura del repositorio

```
eduplay-app/
├── index.html            App completa (HTML, CSS y JavaScript)
├── manifest.json         Permite instalarla en el celular como app
├── js/
│   └── config.js         Configuración de Firebase (opcional)
├── assets/               Logo, íconos y organigrama
└── firebase/
    ├── firestore.rules   Reglas de la base de datos
    └── storage.rules     Reglas para fotos y videos
```

## Cómo usarla

### 1. Probar en el computador

Abre `index.html` en Chrome o Edge. Sin configuración, la app funciona en **modo local**: los usuarios y su progreso quedan guardados en ese navegador.

### 2. Publicar gratis con GitHub Pages

1. Crea un repositorio en GitHub, por ejemplo `eduplay-app`.
2. Sube todo el contenido de esta carpeta, sea con **Add file > Upload files** o con Git:
   ```bash
   git init
   git add .
   git commit -m "EduPlay PMV"
   git branch -M main
   git remote add origin https://github.com/TU-USUARIO/eduplay-app.git
   git push -u origin main
   ```
3. En el repositorio entra a **Settings > Pages** y elige **Branch: main / (root)**.
4. En uno o dos minutos la app queda en `https://TU-USUARIO.github.io/eduplay-app/`.
5. Para instalarla en el celular, ábrela en Chrome para Android y toca **Menú ⋮ > Agregar a la pantalla principal**.

### 3. Conectar la base de datos en la nube (Firebase)

Con Firebase, los usuarios registrados quedan guardados en la nube y pueden entrar desde cualquier celular. También se activan la tabla de clasificación compartida y la subida de fotos y videos.

1. Entra a [console.firebase.google.com](https://console.firebase.google.com) y crea un proyecto, por ejemplo `eduplay-pijino`.
2. En **Authentication > Método de acceso**, activa **Anónimo**.
3. En **Firestore Database**, crea la base de datos en modo producción. Luego, en la pestaña **Reglas**, pega el contenido de `firebase/firestore.rules` y publica.
4. En **Storage**, actívalo y, en la pestaña **Reglas**, pega `firebase/storage.rules`. Storage puede pedir el plan Blaze. Sin Storage, todo funciona menos la subida de fotos y videos; los enlaces sí se guardan.
5. En **Configuración del proyecto > Tus apps**, agrega una app **Web** y copia el objeto `firebaseConfig`.
6. Pégalo en `js/config.js` en lugar de `null`:
   ```js
   window.FIREBASE_CONFIG = {
     apiKey: "AIza...",
     authDomain: "eduplay-pijino.firebaseapp.com",
     projectId: "eduplay-pijino",
     storageBucket: "eduplay-pijino.appspot.com",
     messagingSenderId: "...",
     appId: "..."
   };
   ```
7. En **Authentication > Configuración > Dominios autorizados**, agrega `TU-USUARIO.github.io`.
8. Sube el cambio a GitHub.

Colecciones que crea la app en Firestore:

- `usuarios/{usuario}`: nombre, grado, institución, puntos, insignias, progreso por área, temas vistos e historial de simulacros.
- `recursos/{id}`: tipo (`foto`, `video` o `enlace`), título, área, descripción, URL y autor.

## Tutor IA

En la versión publicada en Claude, el tutor IA explica temas y crea retos nuevos. En GitHub Pages esa función muestra un aviso, porque necesita un servicio de inteligencia artificial con llave privada, y una llave nunca debe ir dentro de una página pública. Para activarlo se puede crear una Cloud Function de Firebase que guarde la llave y responda a la app.

## Tecnologías

- HTML5, CSS3 y JavaScript sin frameworks: la app pesa poco y abre rápido con datos móviles.
- Firebase Authentication (anónima), Cloud Firestore y Storage, con el SDK web 10.12.2.
- Web App Manifest para instalarla en Android.
- Fuentes Fredoka, Figtree y JetBrains Mono (Google Fonts).

## Agregar contenido

Todo el contenido académico está en `index.html`:

- **Temas:** el arreglo `TEMAS`. Cada tema tiene área (`a`), grado (`g`), título (`t`), resumen (`r`) y ejemplo (`ej`).
- **Preguntas:** el arreglo `BANCO`. Cada pregunta tiene área, grado, enunciado (`q`), 4 opciones (`o`), índice de la correcta (`c`, de 0 a 3), explicación (`e`) y, si aplica, un texto de contexto (`ctx`).
- **Enlaces recomendados:** el arreglo `ENLACES`.
- **Fondo del login:** en el CSS, cambia `--foto-login:none;` por `--foto-login:url("assets/fondo-login.jpg");` y guarda la foto en `assets/`.

Las preguntas deben ser validadas por los docentes de cada área antes de usarse con estudiantes.

## Seguridad y datos personales

- Es un prototipo (PMV). Para producción se recomienda migrar el inicio de sesión a Firebase Authentication con correo y contraseña, y endurecer las reglas para que cada estudiante solo pueda modificar su propia cuenta.
- La app guarda solo los datos necesarios. No pide documento, teléfono ni dirección.
- Muchos usuarios son menores de edad: se requiere la autorización del acudiente (Ley 1581 de 2012).

## Equipo

- Instructor: Sergio Andrés Caro Arrieta
- Rector: Prisciliano Lara Mendoza
- Aprendices: Ana Lía Tovar Gutiérrez, Esleidris Catalina Luque Ortiz, Oriana Paba Ortiz, Luisa Fernanda Oliveros López y Jesús Miguel De León Sáenz

## Fuentes de los recursos

- ICFES: cuadernillos de preguntas Saber 11 (2026) y guías de orientación. https://www.icfes.gov.co
- Fundación Empresarios por la Educación (2026): ficha departamental del Magdalena, resultados Saber 11.
- DANE: ENTIC Hogares 2024 e indicadores TIC 2025.
