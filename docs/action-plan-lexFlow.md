# Action Plan – lexFlow (Inmersive Words)

## 🎯 Goal
Crear y lanzar **lexFlow**, una plataforma de aprendizaje de vocabulario inglés basada en inmersión sensorial (imagen → audio → texto) que **utiliza imágenes y videos externos** por defecto y recurre a generación IA solo cuando no se encuentran recursos adecuados.

---

## 🗂️ Execution Roadmap (12 weeks)
| Week | Milestone | Key Activities | Deliverable |
|------|-----------|----------------|------------|
| **1‑2** | **Kick‑off & Foundations** | • Definir visión y métricas de éxito (retención, NPS).<br>• Crear repositorio monorepo (`frontend`, `backend`).<br>• Configurar CI/CD (GitHub Actions), entorno de desarrollo (Docker). | Repo iniciado, pipelines CI verdes. |
| **3‑4** | **Auth & Core Data Model** | • Integrar **Clerk** (social login).<br>• Modelado Prisma: usuarios, conceptos, palabras, progreso, medios (imageUrl, videoUrl).<br>• Migración inicial a PostgreSQL + Redis cache. | Auth funcional, DB schema migrada. |
| **5‑6** | **Content Acquisition Layer** | • Implementar **External Image Service** (Unsplash API + fallback to Pexels).<br>• Implementar **External Video Service** (YouTube/YouEnglish Shorts API / Vimeo o Cloudflare Stream).<br>• Cache de URLs en R2 (metadata, thumbnails). | API de contenido externo lista, caché R2 configurada. |
| **7‑8** | **AI‑fallback Generation** | • Integrar **Stable Diffusion** via Replicate (image generation) – disparado sólo si la búsqueda externa falla.<br>• Integrar **ElevenLabs** (audio TTS) – usado para versiones premium.
• Wrapper que decide: *search → cache → if not found → generate*.
| Generación IA opcional habilitada y probada. |
| **9‑10** | **Lección Inmersiva + Video Shorts** | • UI React: pantalla de lección con **imagen externa**, **audio**, **texto** y **campo de respuesta**.
• Añadir componente **VideoShort** (auto‑play muted, 5‑10 s) antes o después de la imagen cuando el recurso existe.
• Backend endpoints `/api/words/:id` que devuelven `imageUrl`, `videoUrl?`, `audioUrl`.
| Lección MVP con imágenes + videos funcionando. |
| **11‑12** | **Gamificación, SRS & Launch** | • Implementar algoritmo SM‑2 (SRS) y endpoints de progreso.
• Sistema de puntos, niveles y medallas.
• Pruebas unitarias + pruebas de integración (Jest, React Testing Library).
• Despliegue a **Vercel** (frontend) y **Railway** (backend). | MVP listo para beta, métricas de retención configuradas (PostHog). |

---

## 🛠️ Technical Stack
| Layer | Technology | Why |
|-------|------------|-----|
| **Frontend** | **React + TypeScript + Vite** | Desarrollo rápido, tipado, comunidad.
| | **Tailwind CSS** | UI consistente, fácil theming (dark/light).
| | **React‑Query** | manejo sencillo de datos asíncronos (lecciones, SRS).
| | **React‑Player** (or custom `<video>`) | reproducción de shorts/reels.
| **Backend** | **Node.js + Fastify** | API ligera, extensible.
| | **Prisma** (PostgreSQL) | ORM con tipado, migrations.
| | **Redis** | caché de resultados de búsqueda y contadores de puntos.
| **Database** | **PostgreSQL** (Railway) | relaciones complejas (usuarios, conceptos, palabras).
| **Auth** | **Clerk** | login social, gestión de planes free/premium.
| **Media Storage** | **Cloudflare R2** | bajo coste, sin egress para imágenes y videos.
| **External Image API** | **Unsplash / Pexels** (CC0) | amplio catálogo, licencia libre.
| **External Video API** | **YouTube/YouEnglish Shorts API** (public) **or** **Vimeo** / **Cloudflare Stream** | acceso a contenido corto, fácil embed.
| **AI Image Generation** | **Stable Diffusion via Replicate** | fallback cuando no hay imagen externa.
| **AI Audio TTS** | **ElevenLabs** (premium) **or** **Google Cloud TTS** (free tier) | voces naturales, alta calidad.
| **CI/CD** | **GitHub Actions** | lint, tests, builds, despliegue.
| **Hosting** | **Vercel** (frontend) **&** **Railway** (backend) | despliegue rápido, escalable.
| **Analytics** | **PostHog** | funnels, retención, A/B.
| **Monitoring** | **Sentry** | captura de errores.
| **Payments** | **Stripe** | suscripciones premium.
| **Notifications** | **Resend** (email) + **Web‑Push** | recordatorios de práctica.

---

## 📦 Content Strategy (Images & Shorts)
1. **Búsqueda externa primero** – al crear una lección, el backend consulta Unsplash/Pexels por `keyword`. Si la respuesta tiene una imagen con licencia CC0, se usa.
2. **Video Shorts** – se busca en YouTube/YouEnglish Shorts (or Vimeo) con el mismo `keyword`. Se almacena el `embedUrl` y se muestra como contexto visual‑auditivo breve.
3. **Fallback IA** – si no se encuentra ninguna imagen adecuada, se llama a Stable Diffusion para generar una visualización coherente. Lo mismo para audio premium si el TTS gratuito no cubre el idioma/voz.
4. **Caché en R2** – todas las URLs externas se descargan (cuando el acuerdo de licencia lo permite) y se sirven desde R2 para reducir latencia y costos.

---

## ✅ Success Criteria (post‑launch, 4 weeks beta)
- **Retención día‑7** > 40 % de usuarios activos.
- **NPS** ≥ 30.
- **30 %** de lecciones incluyen al menos un video short.
- **< 5 %** de palabras requieren generación IA (demuestra que la estrategia externa funciona).

---

## 📅 Next Steps
1. **Create project repo** (`git init`, push to GitHub).
2. **Set up Cloudflare R2 bucket** & obtain API keys.
3. **Register Unsplash API** (access key) and Vimeo/YouTube/YouEnglish API credentials.
4. **Kick‑off sprint 1** (tasks in the roadmap).

*All milestones are tracked in the internal task board; see `MEMORY.md` for related notes.*
