# Sistema de Análisis de Tendencias en Redes Sociales

Plataforma de microservicios en Python que identifica, analiza y reporta temas trending en múltiples plataformas de redes sociales con segmentación demográfica.

## 📊 Estado del Proyecto

**Fase Actual**: Planificación & Documentación

- ✅ Planificación conceptual completa
- ✅ Estructura de respuesta JSON definida
- ✅ Investigación de herramientas gratuitas completa (10+ APIs gratuitas identificadas)
- ✅ Decisiones de stack técnico tomadas
- ⏳ Implementación técnica pendiente

## 🎯 Descripción General

Este sistema identifica y analiza automáticamente temas trending en plataformas de redes sociales (YouTube, TikTok, Instagram, Facebook) basándose en lineamientos configurables (ej: "finanzas", "tecnología", "salud").

### Características Principales

- **Análisis Multi-Plataforma**: YouTube, TikTok, Instagram, Facebook, Reddit y más
- **Segmentación Demográfica**: Por plataforma, ubicación geográfica, rango de edad y género
- **Procesamiento NLP**: Identificación automática de temas, análisis de sentimiento y reconocimiento de entidades
- **Costo-Efectivo**: Construido con APIs 100% gratuitas (con upgrades pagos opcionales)
- **Insights en Tiempo Real**: Rastrea tendencias emergentes antes de que alcancen su pico

### Casos de Uso

- **Instituciones Financieras**: Adaptar productos según intereses demográficos
- **Creadores de Contenido**: Identificar brechas de contenido y oportunidades
- **Agencias de Marketing**: Diseñar campañas segmentadas por plataforma y demografía
- **Gobierno**: Entender necesidades ciudadanas para políticas públicas

## 🏗️ Arquitectura

### Diseño de Microservicios

```
Petición Usuario → API Gateway (FastAPI)
                         ↓
                 ┌───────┴────────┐
                 ↓                ↓
         Lineamientos Service  Analytics Service
                 ↓
         Message Queue (Celery)
                 ↓
         ┌───────┼───────┬───────┐
         ↓       ↓       ↓       ↓
    YouTube  TikTok  Instagram Facebook
    Collector Collector Collector Collector
         │       │       │       │
         └───────┴───────┴───────┘
                 ↓
         NLP Service (BERTopic)
                 ↓
         PostgreSQL + TimescaleDB
                 ↓
         Redis Cache
```

### Componentes Principales

1. **API Gateway** - Punto de entrada único, autenticación, rate limiting
2. **Lineamientos Service** - Configuración de lineamientos (keywords, hashtags, plataformas)
3. **Collectors** - Recolección de datos específica por plataforma
4. **NLP Service** - Procesamiento de texto, modelado de temas, análisis de sentimiento
5. **Analytics Service** - Agregación de datos y análisis demográfico
6. **Message Queue** - Orquestación de tareas asíncronas (Celery + RabbitMQ/Redis)
7. **PostgreSQL** - Base de datos principal con TimescaleDB para series temporales
8. **Redis** - Capa de caché y message broker

## 📱 Fuentes de Datos

### APIs Gratuitas (MVP)

| Plataforma | API | Demografía | Costo |
|-----------|-----|------------|-------|
| YouTube | Data API v3 | Inferida | GRATIS (10k unidades/día) |
| Instagram | Graph API (Meta) | Acceso completo | GRATIS (200 req/hora) |
| Facebook | Graph API (Meta) | Más completa | GRATIS (200 req/hora) |
| Reddit | PRAW | Estadística | GRATIS |
| Google Trends | pytrends | Geográfica | GRATIS (ilimitado) |
| TikTok | Creative Center | Limitada | GRATIS |
| Mastodon | Mastodon API | No disponible | GRATIS |
| Noticias | NewsAPI.org | N/A | GRATIS (tier dev) |

### Comparación de Costos

- **Este Proyecto (MVP)**: $0-20/mes
- **Brandwatch**: $1,000+/mes
- **Sprinklr**: $2,000+/mes

## 🛠️ Stack Tecnológico

### Backend
- Python 3.11+
- FastAPI (API Gateway)
- Celery (Cola de Tareas)
- PostgreSQL 15+ con TimescaleDB
- Redis (Caché + Message Broker)

### NLP & Procesamiento de Datos
- spaCy (`es_core_news_lg` - Español)
- BERTopic (Modelado de Temas)
- Transformers (BERT)
- Pandas, NumPy

### Integración Redes Sociales
- `google-api-python-client` (YouTube)
- `facebook-sdk` (Instagram/Facebook)
- `praw` (Reddit)
- `pytrends` (Google Trends)
- `Mastodon.py` (Mastodon)
- `newsapi-python` (NewsAPI)
- `snscrape` / `socialreaper` (Scraping)

### Infraestructura
- Docker + Docker Compose
- RabbitMQ o Redis (Message Broker)

## 📈 Estructura de Respuesta API

Todos los endpoints de análisis de tendencias retornan datos en una jerarquía de 4 niveles:

```
Plataforma (youtube, tiktok, instagram, facebook)
  └─ Ubicación Geográfica (país → región → ciudad)
      └─ Rango de Edad (18-24, 25-34, 35-44, etc.)
          └─ Género (masculino, femenino, no_binario, desconocido)
              └─ Temas + Métricas + Ejemplos de Contenido
```

Ver [`documentacion/ESTRUCTURA_JSON_RESPUESTA.md`](documentacion/ESTRUCTURA_JSON_RESPUESTA.md) para el esquema completo.

## 🗄️ Esquema de Base de Datos

### Tablas Principales
- `lineamientos` - Configuración de lineamientos
- `contenido_recolectado` - Contenido sin procesar recolectado
- `temas_identificados` - Temas extraídos por NLP
- `contenido_tema` - Relación Contenido ↔ Temas
- `demografia_tema` - Agregaciones demográficas
- `tendencias` - Datos de series temporales (TimescaleDB)

## 🧠 Pipeline NLP

1. **Limpieza de Texto** - Eliminar stopwords, normalizar, lematizar (spaCy)
2. **Modelado de Temas** - BERTopic para extracción semántica de temas
3. **Reconocimiento de Entidades** - Extraer productos, lugares, personas
4. **Análisis de Sentimiento** - Puntuación de -1 (negativo) a +1 (positivo)
5. **Clasificación** - Relacionar contenido con lineamientos configurados

## 📊 Métricas Clave

- **Tasa de Engagement**: `(Likes + Comentarios + Shares) / Vistas × 100`
- **Trending**: Velocidad de crecimiento > 15% en 24h + mín 50 menciones + engagement sobre promedio
- **Sentimiento**: Rango -1 a +1 (negativo a positivo)
- **Velocidad**: % de crecimiento vs período anterior

## 🚀 Hoja de Ruta de Desarrollo

### Fase 1: MVP (4-6 semanas) - Stack 100% GRATIS
- Configuración de infraestructura (Docker, PostgreSQL, Redis)
- API Gateway básico (FastAPI)
- CRUD de Lineamientos
- 5 Collectors GRATUITOS: Meta Graph API, YouTube, Reddit, Google Trends, Talkwalker
- NLP básico (modelado de temas LDA, spaCy Español)
- API de consulta simple con jerarquía de 4 niveles

### Fase 2: Mejora Multi-Plataforma (4-6 semanas)
- Integración de TikTok Creative Center
- Integración de Mastodon API
- NewsAPI.org para contexto de noticias
- Cola de tareas Celery para procesamiento asíncrono
- NLP avanzado (BERTopic, análisis de sentimiento)
- Servicio de inferencia demográfica
- Respuesta JSON jerárquica completa

### Fase 3: Listo para Producción (4-6 semanas)
- Inferencia demográfica basada en ML
- Series temporales y detección de tendencias
- Alertas en tiempo real
- Exportación de reportes (PDF, Excel)
- Dashboard avanzado
- Optimización de rendimiento
- Testing comprehensivo

## 📚 Documentación

### Planificación Estratégica
- [`PLANIFICACION_CONCEPTUAL.md`](documentacion/PLANIFICACION_CONCEPTUAL.md) - Planificación conceptual completa
- [`ESTRUCTURA_JSON_RESPUESTA.md`](documentacion/ESTRUCTURA_JSON_RESPUESTA.md) - Estructura de respuesta API
- [`HERRAMIENTAS_ANALISIS_TENDENCIAS.md`](documentacion/HERRAMIENTAS_ANALISIS_TENDENCIAS.md) - Análisis de 18 herramientas de marketing/tendencias

### Investigación de Herramientas Gratuitas
- [`HERRAMIENTAS_PLANIFICACION_CONTENIDOS.md`](documentacion/HERRAMIENTAS_PLANIFICACION_CONTENIDOS.md) - Investigación comprehensiva sobre herramientas GRATUITAS de recolección de datos
  - 10 APIs gratuitas identificadas con análisis estratégico
  - Comparación detallada: Alternativas gratuitas vs pagadas
  - Estrategias de inferencia demográfica por plataforma
  - Análisis de costos y estrategias de integración
