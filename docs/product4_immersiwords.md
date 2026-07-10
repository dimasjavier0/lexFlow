# ImmersiWords – Plataforma inmersiva de vocabulario inglés

---

## 1️⃣ Resumen del producto
| Pregunta | Respuesta |
|---|---|
| **Problema** | Los estudiantes de inglés suelen aprender vocabulario mediante **traducción directa al español**, lo que crea una asociación mental frágil y retrasa la fluidez. Además, la mayoría de apps presentan listas planas sin inmersión sensorial (imagen → audio → palabra) y sin un **orden progresivo** basado en conceptos previamente aprendidos. |
| **Usuarios objetivo** | • Estudiantes de inglés (principiantes e intermedios) que buscan una metodología sin traducción.<br>• Profesores de ESL que quieren material inmersivo para sus clases.<br>• Viajeros o expatriados que desean adquirir vocabulario práctico rápidamente. |
| **Propuesta de valor** | "Aprende palabras **por inmersión** – primero una **imagen** (sin texto), luego el **audio nativo**, y solo al final la **palabra escrita**. El orden sigue una **red de conceptos** que parte de lo que ya sabes, garantizando conexiones fuertes y una progresión natural."
| **Ventaja competitiva** | 1️⃣ **Secuencia inmersiva** (imagen → audio → texto) basada en neurociencia del aprendizaje de segunda lengua.<br>2️⃣ **Mapa conceptual progresivo** que evita palabras aisladas.
3️⃣ **Gamificación ligera**: puntos, niveles y medallas para reforzar la práctica diaria.
4️⃣ **Contenido propio** (imágenes libres de derechos, audios de hablantes nativos). |

---

## 2️⃣ Validación de mercado
| Competidor | Tipo | Qué hacen bien | Qué hacen mal | Oportunidad |
|---|---|---|---|---|
| Duolingo | Directo | Lecciones cortas, gamificación, gran base de usuarios. | Vocabulario presentado con **traducción al idioma nativo**; falta de inmersión visual‑auditiva. |
| Memrise | Directo | Utiliza *mems* visuales y videos de nativos. | Depende de usuarios que crean sus propios mems → calidad variable; sigue usando traducción textual. |
| Anki | Indirecto | Sistema de tarjetas espaciadas (SRS). | No provee inmersión sensorial ni estructura de conceptos. |
| Rosetta Stone | Directo | Inmersión total sin traducción. | Precio alto, UI anticuada, foco en suscripciones a gran escala. |
| Quizlet | Indirecto | Flashcards y modos de juego. | Permite traducción, sin enfoque progresivo de conceptos. |

**Riesgos de mercado**
- **Preferencia por apps gratuitas** (Duolingo), lo que dificulta la adopción si el precio es alto.
- **Creación de contenido** (imágenes + audios nativos) es costosa y requiere licencias libres de derechos.
- **Retención**: si la progresión no está bien diseñada, los usuarios pueden abandonar.
- **Eficacia pedagógica**: sin evidencia clara de que la inmersión sin traducción mejore resultados, la propuesta podría ser percibida como moda.

---

## 3️⃣ MVP (Minimum Viable Product)
| Tipo | Funcionalidades |
|---|---|
| **Obligatorias** | 1. Registro / login.<br>2. **Mapa conceptual** inicial (ej. "Objetos de la casa") con palabras desbloqueables.
3. **Lección inmersiva**: muestra imagen (sin texto) → reproduce audio nativo → revela la palabra escrita y permite al usuario escribirla.
4. **Sistema de repaso espaciado (SRS)** por cada palabra.
5. **Puntos y niveles** (gamificación básica).
6. **Progreso y medallas** (p. ej., "30‑day streak"). |
| **Opcionales** | • Modo "Quiz" de selección múltiple para reforzar.<br>• Export de listas a CSV para usuarios avanzados. |
| **No construir ahora** | • IA que genera imágenes a partir de la palabra (Stable Diffusion) – sólo cuando no existan imágenes externas adecuadas.
• Videos cortos (shorts/reels) para contexto de palabras – usar videos externos, generación interna solo si no hay disponible.<br>• Integración de *speech‑to‑text* para que el usuario practique pronunciación.
• Marketplace de tutores en vivo.<br>• Funcionalidad de "viajes" (vocabulario por país). |

---

## 4️⃣ Roadmap por versiones
| Versión | Duración (semanas) | Funcionalidades clave | Motivo |
|---|---|---|---|
| **MVP** | 1‑9 | Registro, mapa conceptual inicial, lección inmersiva (imagen → audio → palabra), SRS básico, puntos/niveles, medallas. | Validar que la secuencia inmersiva genera mayor retención que métodos tradicionales. |
| **v1.1** | 10‑14 | Modo **quiz** (selección múltiple), estadísticas de rendimiento (aciertos, tiempo), integración con **Google Drive** para backup. | Aumentar engagement y ofrecer métricas útiles a los usuarios y profesores. |
| **v2.0** | 15‑22 | **Creación de contenidos colaborativa**: usuarios pueden proponer imágenes y audios (moderados).<br>Gamificación avanzada: retos semanales, rankings globales, logros.
| Refuerzo de la **red conceptual** (desbloqueo de conceptos hijos al completar padres). |
| **v3.0** | 23‑34 | **IA Generativa**: generación automática de imágenes (Stable Diffusion) y audios (ElevenLabs) para palabras sin contenido.
Integración de **speech‑to‑text** para evaluar pronunciación del usuario.
Marketplace de tutores (sesiones 1‑a‑1). |

---

## 5️⃣ Arquitectura técnica
| Capa | Tecnologías (justificación) |
|---|---|
| **Frontend** | **React + TypeScript + Vite** – rapidez, tipado, comunidad.<br>**Tailwind CSS** – UI consistente y responsiva.<br>**React‑Query** – manejo de datos asíncronos (lecciones, SRS). |
| **Backend** | **Node .js + Fastify** – API ligera, fácil de escalar.
**Prisma** ORM sobre **PostgreSQL** – modelo relacional (usuarios, palabras, progresos, mapas de conceptos). |
| **Base de datos** | **PostgreSQL** – relaciones entre usuarios, palabras, conceptos y logs de repaso.
**Redis** – caché de sesiones y contadores de puntos. |
| **Auth** | **Clerk** – login social (Google, Apple) y gestión de planes (free / premium). |
| **Almacenamiento de medios** | **Cloudflare R2** – imágenes y audios (bajo coste, sin egress). |
| **Audio TTS (v3.0)** | **ElevenLabs API** o **Google Cloud Text‑to‑Speech** – voces naturales, alta calidad.
| **IA (v3.0)** | **Stable Diffusion** (via Replicate) para generar imágenes bajo demanda.
**Claude / GPT‑4o** para generar descripciones y posibles *mems*.
| **Servicios en la nube** | **Railway.app** (backend) – despliegue rápido, escalable.
**Vercel** (frontend). |
| **CI/CD** | **GitHub Actions** – lint, pruebas, builds, despliegue. |
| **Monitoreo** | **Sentry** (errores), **UptimeRobot** (pings). |
| **Analítica** | **PostHog** – funnels (registro → lección completada), retención diaria, conversión a premium. |
| **Notificaciones** | **Resend** (emails de streak y logros) + **Web‑Push** para recordatorios de práctica. |
| **Pago** | **Stripe** – suscripciones mensuales y pagos únicos para packs de contenido premium. |

---

## 6️⃣ Modelo de datos (conceptual)
```
Usuario
 ├─ id, email, nombre, plan (free / premium), streak, puntos, nivel
 └─ relaciones: 1→N ProgresoPalabra, 1→N LogLeccion, 1→N LogActividad

Concepto
 ├─ id, nombre, descripcion, parentId (nullable)
 ├─ relaciones: 1→N Palabra

Palabra
 ├─ id, texto, audioUrl, imageUrl, nivelDificultad
 ├─ conceptoId (FK a Concepto)
 └─ relaciones: 1→N ProgresoPalabra

ProgresoPalabra
 ├─ id, usuarioId, palabraId, ultimaRepeticion, factor (SM‑2), intervalo, repeticionesCorrectas
 └─ status (new / learning / mastered)

LogLeccion
 ├─ id, usuarioId, palabraId, timestamp, correcto (bool), tiempoRespuesta
 └─ metadata opcional (audioPlayCount)

LogActividad
 ├─ id, usuarioId, tipo (punto, nivel, logro), detalle, timestamp
```

---

## 7️⃣ APIs principales (REST)
| Recurso | Método | Endpoint | Descripción |
|---|---|---|---|
| **Auth** | POST | `/api/auth/signup` | Registro. |
| | POST | `/api/auth/login` | Login, devuelve JWT. |
| **Conceptos** | GET | `/api/concepts` | Lista de conceptos (árbol). |
| | GET | `/api/concepts/:id` | Detalle de concepto + palabras asociadas. |
| **Palabras** | GET | `/api/words/:id` | Obtiene datos de palabra (audio, imagen). |
| | POST | `/api/words/:id/learn` | Marca intento de aprendizaje, devuelve audio y espera input del usuario. |
| **Progreso** | POST | `/api/users/me/progress/:wordId` | Registra repaso (SM‑2). |
| | GET | `/api/users/me/progress` | Estado de todas las palabras del usuario (para SRS). |
| **Lecciones** | POST | `/api/lessons/start` | Genera lección basada en conceptos desbloqueados.
| | POST | `/api/lessons/:id/complete` | Registra resultado (correcto/incorrecto). |
| **Logros** | GET | `/api/users/me/achievements` | Lista logros y medallas. |
| **Billing** | POST | `/api/billing/subscribe` | Suscripción premium (Stripe). |
| **Admin – Content** | POST | `/api/admin/words` | Añadir nueva palabra (audio + imagen). (solo admins). |

---

## 8️⃣ Diseño UX
| Pantalla | Descripción |
|---|---|
| **Landing** | Hero con tagline "Aprende vocabulario sin traducir". CTA "Empieza gratis" y breve video que muestra la secuencia inmersiva. |
| **Onboarding** | 3‑step wizard: 1) Seleccionar nivel inicial; 2) Elegir tema (ej. "Casa", "Comida"); 3) Crear avatar. |
| **Dashboard** | Mapa de conceptos (árbol colapsable). Cada nodo muestra número de palabras desbloqueadas / totales. Botón "Comenzar lección" en nodo activo. |
| **Lección inmersiva** | Pantalla a pantalla completa:<br>1️⃣ Imagen grande sin texto (5 s).<br>2️⃣ Reproducción de audio nativo (pronunciación).<br>3️⃣ Aparece la palabra escrita y campo de input para que el usuario la escriba.<br>Feedback instantáneo (check / cross). |
| **Quiz (v1.1)** | Después de 5 palabras, mini‑quiz de selección múltiple para reforzar. |
| **Progreso / Estadísticas** | Barra de nivel, puntos acumulados, streak diario, gráfico de SRS (palabras aprendidas por día). |
| **Logros** | Tarjetas de medallas (ej. "30‑day streak", "Primer 100 palabras"). |
| **Perfil** | Avatar, nivel, puntos, historial de lecciones, opción de upgrade a premium. |
| **Navegación** | Sidebar (desktop) / Bottom nav (mobile): Dashboard | Lección | Quiz | Logros | Perfil. |

**Experiencia clave**: El usuario **no ve la traducción** hasta después de haber asociado imagen + sonido; esto crea una conexión neuronal más fuerte y reduce la dependencia del español. |

---

## 9️⃣ Monetización
| Estrategia | Detalle | Recomendación |
|---|---|---|
| **Freemium** | Gratis: acceso a 3 conceptos, 30 lecciones/día, audio de calidad 128 kbps, sin export.
Premium $6.99 / mes: conceptos ilimitados, audio 320 kbps, imágenes de alta resolución, estadísticas avanzadas, modo offline. |
| **Suscripción familiar** | $12.99 / mes para hasta 4 cuentas (ideal para familias). |
| **Marketplace de contenidos premium** | Packs de vocabulario temático (ej. "Vocabulario médico") con imágenes y audios creados por profesionales; comisión 20 %. |
| **Licencia escolar** | Precio por estudiante para instituciones (descuentos por volumen). |
| **Publicidad** | Banners de editoriales de aprendizaje de idiomas (solo en versión free). |
| **Compra única** | "Pack de 500 lecciones premium" $14.99. |
| **Donaciones** | Botón "Invita un café" a los fundadores. |

**Modelo recomendado**: lanzar **Freemium + suscripción premium** desde v1.1; agregar marketplace y licencias escolares después de validar adopción institucional. |

---

## 🔟 Riesgos
| Tipo | Riesgo | Mitigación |
|---|---|---|
| **Técnico** | Generación de audio de alta calidad a escala (coste). | Usar TTS de alta calidad solo para usuarios premium; cachear audios. |
| **Legal** | Derechos de imagen y audio (licencias). | Utilizar imágenes **CC0** (Unsplash, Pexels) y audios de hablantes nativos con acuerdos de licencia; indicar fuentes. |
| **Escalabilidad** | SRS y cálculo de intervalos para millones de usuarios. | Índices en PostgreSQL (`user_id + next_review`), caché Redis para próximos repasos. |
| **Adquisición** | Competencia fuerte de Duolingo y Memrise. | Enfocar en **nicho inmersivo sin traducción** y **educadores** que busquen material de calidad.
| **Monetización** | Usuarios free pueden estar satisfechos sin premium. | Limitar conceptos y lecciones diarias; ofrecer valor claro en premium (audio 320 kbps, imágenes HD, stats). |
| **Eficacia pedagógica** | Si no se demuestra mejora, la propuesta pierde credibilidad. | Realizar pruebas A/B con usuarios y publicar resultados (p.ej., mayor retención del vocabulario). |
| **Dependencia de terceros** | Cambios en precios o disponibilidad de APIs TTS/Imagen. | Arquitectura modular: poder cambiar a otro proveedor (Azure TTS, Stability AI). |

---

## 1️⃣ Plan de desarrollo (1 desarrollador)
| Semana | Objetivo | Funcionalidades entregadas | Entregable |
|---|---|---|---|
| **1‑2** | Setup base | Repo, CI (GitHub Actions), React + Vite scaffold, Dockerfile. |
| **3‑4** | Auth y DB | Integración Clerk, modelo Prisma (User, Concept, Word, Progreso, Log). |
| **5‑6** | Mapa conceptual UI | Árbol interactivo con D3.js (React‑D3), carga de conceptos seed. |
| **7‑8** | Lección inmersiva (MVP) | Mostrar imagen, reproducir audio (Google TTS), revelar palabra, input de respuesta, feedback. |
| **9‑10** | SRS Backend | Implementar algoritmo SM‑2, endpoint `/users/me/progress/:wordId`. |
| **11‑12** | Gamificación base | Puntos, barra de nivel, medallas de streak. |
| **13‑14** | Freemium limits + Billing | Cuota de 3 conceptos, suscripción Stripe $6.99. |
| **15‑16** | Quiz & Stats (v1.1) | Mini‑quiz de selección múltiple, página de estadísticas (aciertos, tiempo). |
| **17‑18** | Beta launch + Analytics | Deploy a Vercel + Railway, integrar PostHog, recopilar métricas de retención. |
| **19‑20** | Contenido colaborativo | UI para que admins/submissions añadan nuevas palabras (audio + imagen). |
| **21‑22** | Rankings & retos (v2.0) | Leaderboard global, retos semanales, desbloqueo de conceptos hijos. |
| **23‑26** | IA generación (v3.0) | Integrar Stable Diffusion (via Replicate) para imágenes on‑demand; ElevenLabs TTS para audio premium. |
| **27‑30** | Speech‑to‑text & evaluación (v3.0) | Integrar reconocimiento de voz (Web Speech API) para que el usuario practique pronunciación; feedback de precisión. |
| **31‑34** | Marketplace & tutores (v3.0) | Catálogo de packs premium, pagos vía Stripe, sistema de reservas de tutores. |

**Criterio de "listo"**: pruebas automatizadas pasan, 10 usuarios beta completan al menos 20 lecciones, retención >40 % al día 7. |

---

## 1️⃣ Despliegue económico (MVP)
| Paso | Acción | Herramienta / costo |
|---|---|---|
| **Código** | GitHub repo privado | Gratis |
| **CI/CD** | GitHub Actions (lint, tests, build, deploy) | Gratis (≤ 2 000 min/mes) |
| **Frontend** | Vercel (Plan Hobby) | Gratis (500 GB bw/mes) |
| **Backend** | Railway (Starter) – 500 h, 1 GB DB, 1 GB R2 | Gratis dentro del límite |
| **DB** | PostgreSQL (Railway) | Gratis |
| **Almacenamiento** | Cloudflare R2 (10 GB gratis) | Gratis |
| **Auth** | Clerk (Free ≤ 5 k MAU) | Gratis |
| **Audio TTS (free tier)** | Google Cloud Text‑to‑Speech (first 1 M chars gratis) | Gratis |
| **Billing** | Stripe (solo comisión por transacción) | $0 + 2.9 % + ¢ |
| **Monitoreo** | Sentry (Free) | Gratis |
| **Analytics** | PostHog (Free) | Gratis |
| **Emails** | Resend (Free 100 emails/mes) | Gratis |

Con esta pila, el **costo mensual** del MVP es **casi nulo**, salvo comisiones de Stripe cuando haya suscripciones.