# PLANIFICACIÓN CONCEPTUAL
## Sistema de Análisis de Tendencias en Redes Sociales

---

## 1. OBJETIVO DEL PROYECTO

### 1.1 Propósito General
Desarrollar un sistema automatizado que identifique, analice y reporte tendencias temáticas en redes sociales basándose en lineamientos específicos (ejemplo: finanzas, tecnología, salud), con segmentación demográfica detallada por plataforma, ubicación geográfica, rango de edad y género.

### 1.2 Problema a Resolver
Las organizaciones, marcas y creadores de contenido necesitan entender:
- ¿Qué temas están conversando las personas en redes sociales sobre un área específica?
- ¿Qué diferencias existen por plataforma social?
- ¿Cómo varían los intereses según demografía (edad, género, ubicación)?
- ¿Qué contenido genera más engagement en cada segmento?

### 1.3 Casos de Uso Reales

**Ejemplo 1: Lineamiento "Finanzas"**
- YouTube: Identificar que jóvenes de 18-24 buscan "Cómo invertir en la bolsa de valores de Colombia"
- TikTok: Detectar tendencia viral sobre "Cómo generar un plan de ahorros" entre millennials
- Instagram/Facebook: Rastrear conversaciones sobre "Cómo acceder a subsidios para compra de casa" en regiones específicas

**Ejemplo 2: Aplicaciones**
- **Instituciones financieras**: Adaptar productos según intereses por segmento demográfico
- **Creadores de contenido**: Identificar gaps de contenido y oportunidades
- **Agencias de marketing**: Diseñar campañas segmentadas por plataforma y demografía
- **Gobiernos**: Entender necesidades ciudadanas en políticas públicas

---

## 2. ANÁLISIS DE REDES SOCIALES Y APIS

### 2.1 YouTube

#### ¿QUÉ es YouTube en el contexto de este análisis?
Plataforma de video más grande del mundo, ideal para contenido educativo, tutoriales y contenido de formato largo. Usuario típico busca aprender en profundidad sobre temas específicos.

#### ¿QUÉ API utilizaremos?
**YouTube Data API v3** (https://developers.google.com/youtube/v3)

#### ¿PARA QUÉ sirve esta API?
- Buscar videos por palabras clave relacionadas con el lineamiento
- Obtener información completa de videos: título, descripción, estadísticas (vistas, likes, comentarios)
- Acceder a comentarios de usuarios para análisis de conversación
- Obtener datos de canales y su categorización
- Identificar videos trending por región

#### ¿POR QUÉ elegir esta API?
1. **Oficialidad y confiabilidad**: API oficial de Google, datos precisos y actualizados
2. **Cobertura**: Acceso a todo el catálogo público de YouTube
3. **Costo**: Cuota gratuita generosa (10,000 unidades/día = aprox. 3,000 búsquedas)
4. **Filtrado geográfico**: Permite filtrar por región para segmentación
5. **Datos estructurados**: Respuestas JSON bien documentadas
6. **Métricas completas**: Engagement detallado (vistas, likes, comentarios, shares)

#### Datos que obtendremos
- Título y descripción de videos (para topic modeling)
- Métricas de engagement: vistas, likes, dislikes, comentarios
- Fecha de publicación
- Canal/creador
- Región relevante del video
- Comentarios de usuarios (opcional, consume cuota adicional)

#### Limitaciones identificadas
- No proporciona demografía directa de la audiencia (necesitamos YouTube Analytics API que requiere ser propietario del canal)
- Cuota diaria limitada (mitigable con múltiples API keys o cache)
- Información demográfica debe inferirse por otros métodos

---

### 2.2 TikTok

#### ¿QUÉ es TikTok en el contexto de este análisis?
Plataforma de videos cortos (15-60 segundos), líder en tendencias virales. Audiencia joven (Gen Z y Millennials). Ideal para detectar tendencias emergentes rápidamente.

#### ¿QUÉ API utilizaremos?
**TikTok Research API** (https://developers.tiktok.com/products/research-api/)

#### ¿PARA QUÉ sirve esta API?
- Buscar videos por hashtags y palabras clave
- Obtener datos de engagement: vistas, likes, shares, comentarios
- Acceder a información de usuarios y creadores
- Filtrar por región y fecha
- Analizar trending hashtags

#### ¿POR QUÉ elegir esta API?
1. **Velocidad de tendencias**: TikTok es donde nacen las tendencias virales
2. **Demografía joven**: Acceso a insights de Gen Z y Millennials
3. **Datos de engagement**: Métricas completas de interacción
4. **Filtrado regional**: Permite segmentar por país/región
5. **Análisis de hashtags**: Fundamental para identificar temas trending

#### Datos que obtendremos
- Título/descripción de videos
- Hashtags utilizados
- Métricas: vistas, likes, comentarios, shares, guardados
- Información del creador
- Región/país (si disponible)
- Audio/música utilizada (contexto adicional)

#### Limitaciones identificadas
- Requiere aprobación para TikTok Research API (proceso puede tardar)
- Solo datos de últimos 30 días
- Demografía específica limitada

#### Alternativa para MVP
**TikTokApi (Python library no oficial)**: Permite prototipado rápido sin aprobación, con funcionalidad limitada pero suficiente para prueba de concepto.

#### Herramienta Complementaria: TikTok Creative Center

**¿QUÉ es?**: Herramienta oficial y GRATUITA de TikTok para análisis de tendencias, creatividad y hashtags.
(https://ads.tiktok.com/business/creativecenter)

**¿PARA QUÉ sirve?**:
- Dashboard de hashtags trending con volumen de posts y vistas
- Filtrado por industria/categoría (ej: "finanzas", "tecnología")
- Análisis de audios/músicas trending
- Datos de trending creators por nicho
- Métricas de 7, 30 o 90 días

**¿POR QUÉ es valiosa?**:
1. **Gratuita y oficial**: Herramienta de TikTok con datos 100% precisos
2. **Trending en tiempo real**: Actualización constante de hashtags populares
3. **Filtrado por vertical**: Permite enfocarse solo en temas del lineamiento
4. **Audio-driven**: TikTok es muy dependiente de audio, esta herramienta identifica audios virales
5. **Complementa TikTok API**: Mientras se obtiene aprobación para Research API, Creative Center funciona inmediatamente

**Cómo la integraremos**:
- Monitoreo diario automatizado de hashtags trending en categorías relevantes
- Complementar datos de TikTok Research API con insights de Creative Center
- Identificar audios virales asociados a temas del lineamiento

---

### 2.3 Instagram

#### ¿QUÉ es Instagram en el contexto de este análisis?
Plataforma visual (fotos, carruseles, reels, stories), enfocada en lifestyle, influencers y marcas. Audiencia diversa con buen balance demográfico.

#### ¿QUÉ API utilizaremos?
**Instagram Graph API** (parte de Meta Graph API)
(https://developers.facebook.com/docs/instagram-api)

#### ¿PARA QUÉ sirve esta API?
- Buscar publicaciones por hashtags
- Obtener posts recientes de hashtags específicos
- Analizar métricas de engagement: likes, comentarios, guardados, shares
- Acceder a Instagram Insights con demografía de audiencia (para cuentas autorizadas)
- Obtener información de ubicación de posts

#### ¿POR QUÉ elegir esta API?
1. **Datos demográficos oficiales**: Instagram Insights proporciona edad, género y ubicación de audiencia
2. **Alta calidad de segmentación**: Usuarios de Instagram suelen tener perfiles completos
3. **Hashtags como organizadores**: Cultura de hashtags facilita identificación de temas
4. **Integración con Facebook**: Permite análisis cruzado entre plataformas
5. **API madura y bien documentada**: Parte del ecosistema Meta

#### Datos que obtendremos
- Caption/texto de publicaciones
- Hashtags utilizados
- Tipo de contenido: foto, video, carousel, reel
- Métricas: likes, comentarios, guardados, shares
- Ubicación geográfica (si está taggeada)
- **Demografía de audiencia**: edad, género, ubicación (para cuentas con acceso)

#### Limitaciones identificadas
- Requiere cuenta de negocio o creador para acceso completo
- Solo acceso a cuentas propias o con autorización OAuth
- Rate limits: 200 llamadas/hora por usuario
- Búsqueda por hashtag limitada (solo hashtags públicos recientes)

---

### 2.4 Facebook

#### ¿QUÉ es Facebook en el contexto de este análisis?
Red social generalista, más grande del mundo. Audiencia más adulta que otras plataformas. Ideal para grupos, páginas temáticas y discusiones comunitarias.

#### ¿QUÉ API utilizaremos?
**Meta Graph API** (https://developers.facebook.com/docs/graph-api)

#### ¿PARA QUÉ sirve esta API?
- Acceder a publicaciones de páginas públicas
- Analizar grupos públicos (limitado)
- Obtener métricas de engagement: reacciones, comentarios, shares
- Acceder a Facebook Insights con demografía detallada
- Búsqueda de contenido público (limitada desde 2018)

#### ¿POR QUÉ elegir esta API?
1. **Mejor demografía del mercado**: Facebook tiene los datos demográficos más completos y precisos
2. **Audiencia madura**: Acceso a segmentos de 35+ años, difíciles en otras plataformas
3. **Grupos temáticos**: Conversaciones profundas en grupos especializados
4. **Páginas especializadas**: Acceso a contenido de expertos y organizaciones
5. **Integración con Instagram**: Análisis unificado del ecosistema Meta

#### Datos que obtendremos
- Texto completo de publicaciones
- Tipo de contenido: texto, foto, video, link, evento
- Métricas: reacciones (like, love, wow, sad, angry), comentarios, shares
- **Demografía completa**: edad, género, ubicación, nivel educativo, intereses
- Información de páginas y grupos
- Fecha y hora de publicación

#### Limitaciones identificadas
- Búsqueda pública muy limitada desde Cambridge Analytica (2018)
- Requiere permisos de páginas para acceso completo a insights
- Rate limits: 200 llamadas/hora por usuario
- Contenido privado no accesible (solo público)

---

### 2.5 Google Trends

#### ¿QUÉ es Google Trends en el contexto de este análisis?
Herramienta gratuita de Google que muestra el volumen de búsquedas de términos específicos a lo largo del tiempo y por geografía. Representa la **intención activa** de los usuarios, no solo el consumo pasivo de contenido.

#### ¿QUÉ herramienta utilizaremos?
**Google Trends** (https://trends.google.com)
Acceso programático vía **pytrends** (biblioteca Python no oficial) o **Serpapi** (servicio de pago)

#### ¿PARA QUÉ sirve esta herramienta?
- Ver evolución temporal del interés en un tema (últimas horas, días, meses, años)
- Comparar múltiples términos de búsqueda simultáneamente
- **Interest by Region**: Interés por país, región, ciudad (geografía precisa)
- **Related Queries**: Búsquedas relacionadas y "rising queries" (términos con crecimiento explosivo)
- **Trending Searches**: Búsquedas más populares del momento en tiempo real
- Datos históricos desde 2004 (más de 15 años)

#### ¿POR QUÉ es CRÍTICA para el proyecto?
1. **Captura intención directa**: La gente busca activamente información, no solo consume contenido sugerido por algoritmos
2. **Predicción de demanda**: Detecta interés creciente 2-4 semanas ANTES de que explote en redes sociales
3. **Validación de tendencias**: Confirma si una tendencia en redes sociales es real o solo una burbuja algorítmica
4. **Detección de gaps de contenido**: Alta búsqueda + Bajo contenido en redes = OPORTUNIDAD masiva
5. **Estacionalidad**: 15+ años de histórico revelan patrones anuales (ej: "declaración de renta" pico en abril cada año)
6. **Geografía precisa**: Datos hasta nivel de ciudad, identifica dónde hay más interés real
7. **Related queries**: Descubre temas adyacentes automáticamente (expansión de lineamientos)
8. **100% gratuita**: Sin costos, sin límites reales para uso razonable

#### Datos que obtendremos
- **Interest Over Time**: Escala 0-100 del interés por período (hora, día, semana, mes, año)
- **Interest by Region**: Desglose geográfico (país → región → ciudad)
- **Related Topics**: Temas relacionados con puntuación de correlación
- **Related Queries (Top)**: Búsquedas más comunes relacionadas
- **Related Queries (Rising)**: Búsquedas con crecimiento explosivo (+100%, +250%, +500%)
- **Trending Searches**: Qué es trending en tiempo real por país

#### Casos de uso específicos en el proyecto

**Ejemplo 1: Detección de Gap de Contenido**
```
Google Trends: "crédito hipotecario colombia"
- Volumen: 10,000+ búsquedas/mes
- Tendencia: +40% último mes
- Región: 100% Bogotá, 75% Medellín

YouTube API: Solo 15 videos sobre el tema
Instagram: 45 posts en último mes

INSIGHT: OPORTUNIDAD MASIVA - Alta demanda de información,
         bajo supply de contenido. Gap de mercado.
```

**Ejemplo 2: Validación de Tendencia Real vs Burbuja**
```
TikTok API: "plan de ahorros 50/30/20"
- Trending: +200% engagement
- 1.2M videos última semana

Google Trends: "método 50/30/20"
- Búsquedas: +180% última semana
- Rising query en 15 países

INSIGHT: TENDENCIA REAL VALIDADA - No es solo viral por
         algoritmo, hay interés genuino de búsqueda.
```

**Ejemplo 3: Predicción Temprana**
```
Semana 1 - Google Trends: "invertir en oro" +60% búsquedas
Semana 2 - Google Trends: +120% búsquedas
Semana 3 - YouTube/TikTok: Aparecen videos masivos sobre oro
Semana 4 - Tendencia mainstream en todas las redes

INSIGHT: Google Trends detectó la tendencia 2-3 semanas
         ANTES de que explotara en redes sociales.
```

**Ejemplo 4: Estacionalidad y Planificación**
```
Google Trends histórico (2015-2024): "subsidio vivienda"
- Pico cada año: Enero-Febrero
- Valle: Junio-Agosto

INSIGHT: Intensificar recolección y análisis en Nov-Dic
         para capturar contenido antes del pico de interés.
```

#### Limitaciones identificadas
- **No hay API oficial**: Dependemos de pytrends (scraping) o Serpapi (pago)
- **Escala relativa**: Valores 0-100 son relativos al pico, no números absolutos
- **Sin demografía directa**: No muestra edad/género de quien busca (pero se puede inferir por temas)
- **Rate limits informales**: Google puede bloquear temporalmente si se abusa (mitigable con delays entre requests)
- **Datos agregados**: Google agrupa variaciones similares automáticamente
- **Volatilidad en queries de bajo volumen**: Términos con pocas búsquedas pueden mostrar datos erráticos

#### Integración con el proyecto

**Rol 1: Validador de Tendencias**
- Cada tema identificado en redes sociales se cruza con Google Trends
- Si coincide = Alta confianza (tendencia real)
- Si no coincide = Posible burbuja algorítmica (investigar más)

**Rol 2: Detector de Oportunidades**
- Keywords con alto volumen en Google Trends
- Bajo contenido en redes sociales
- = Gap de contenido → Oportunidad para creadores

**Rol 3: Predictor Temprano**
- Monitoreo semanal de rising queries en categorías del lineamiento
- Alertas cuando un término crece >100% en búsquedas
- Permite anticipar qué será trending antes que la competencia

**Rol 4: Contexto Geográfico**
- Validar que interés geográfico en Google coincide con contenido de redes
- Identificar regiones con alto interés pero bajo contenido local

**Nueva estructura en respuesta JSON**:
```json
{
  "busquedas_google": {
    "colombia": {
      "keywords_top": [
        {
          "keyword": "invertir en bolsa",
          "interes": 100,
          "crecimiento_semanal": "+25%",
          "regiones_top": ["Bogotá", "Medellín"],
          "related_rising": ["broker colombia", "como invertir dinero"]
        }
      ]
    }
  },
  "insights_correlacion": [
    {
      "tipo": "search_content_gap",
      "descripcion": "Alto volumen de búsqueda 'crédito hipotecario' (+40%) pero bajo contenido en YouTube"
    },
    {
      "tipo": "trend_validation",
      "descripcion": "Tendencia TikTok 'plan de ahorros' validada por Google Trends (+180%)"
    }
  ]
}
```

---

### 2.6 Twitter/X (Opcional)

#### ¿QUÉ es Twitter/X en el contexto?
Plataforma de microblogging, red de noticias y conversaciones en tiempo real. Ideal para detectar tendencias emergentes y análisis de sentiment.

#### ¿QUÉ API utilizaremos?
**Twitter API v2** (https://developer.twitter.com/en/docs/twitter-api)

#### ¿PARA QUÉ sirve?
- Búsqueda en tiempo real de tweets
- Streaming de tweets (análisis en vivo)
- Análisis de trending topics
- Datos de usuarios e influencers
- Geolocalización de tweets

#### ¿POR QUÉ incluirla? (Opcional)
1. **Tiempo real**: La plataforma más rápida para detectar tendencias
2. **Conversaciones públicas**: Todo es público por defecto
3. **Hashtags y threads**: Fácil seguimiento de temas
4. **Influencers y expertos**: Acceso a líderes de opinión

#### Limitaciones
- Nivel gratuito muy limitado (solo 1,500 tweets/mes desde cambios de 2023)
- Academic Research track requiere aprobación
- Costo elevado para nivel empresarial
- Demografía inferida, no directa

**Decisión**: Dejar como opcional/futuro, priorizar las 4 principales.

---

## 3. ARQUITECTURA CONCEPTUAL DEL SISTEMA

### 3.1 Visión General

El sistema se construirá como una arquitectura de **microservicios** para lograr:
- **Escalabilidad**: Cada componente puede escalar independientemente
- **Mantenibilidad**: Cambios en una red social no afectan las demás
- **Resiliencia**: Si un servicio falla, los demás continúan operando
- **Flexibilidad**: Fácil agregar nuevas plataformas o funcionalidades

### 3.2 Componentes Principales

#### **1. API Gateway**
**¿Qué es?**: Punto de entrada único al sistema.
**¿Para qué sirve?**:
- Recibir todas las peticiones de clientes
- Autenticar y autorizar usuarios
- Enrutar peticiones a servicios internos
- Implementar rate limiting
- Proporcionar documentación API (Swagger/OpenAPI)

**¿Por qué necesitamos esto?**:
- Simplifica el acceso para clientes (una sola URL)
- Centraliza seguridad y logging
- Permite cambiar servicios internos sin afectar clientes

---

#### **2. Servicio de Lineamientos**
**¿Qué es?**: Gestor de configuración de lineamientos a analizar.

**¿Para qué sirve?**:
- CRUD de lineamientos (crear, leer, actualizar, eliminar)
- Definir keywords y hashtags por lineamiento
- Configurar qué plataformas rastrear
- Establecer frecuencia de recolección

**¿Por qué separarlo?**:
- Permite gestionar múltiples lineamientos simultáneamente
- Los usuarios pueden configurar sin tocar código
- Facilita agregar nuevos lineamientos dinámicamente

**Ejemplo de lineamiento**:
```
Lineamiento: "Finanzas"
Keywords: ["inversión", "ahorro", "crédito", "subsidio", "bolsa de valores"]
Hashtags: ["#finanzaspersonales", "#ahorro", "#inversion", "#creditohipotecario"]
Plataformas: ["youtube", "tiktok", "instagram", "facebook"]
Frecuencia: cada 6 horas
```

---

#### **3. Collectors (Recolectores por Plataforma)**

**¿Qué son?**: Servicios especializados, uno por cada red social.

**¿Para qué sirven?**:
- Conectar con la API de su red social específica
- Recolectar datos según lineamientos configurados
- Normalizar datos a formato común
- Manejar rate limits específicos de cada API
- Reintentar en caso de errores

**¿Por qué un collector por plataforma?**:
- Cada API tiene particularidades diferentes
- Rate limits distintos por plataforma
- Permite escalar collectors independientemente según volumen
- Fácil mantenimiento (cambios en una API no afectan otros collectors)

**Collectors necesarios**:
1. **YouTube Collector**: Busca videos, extrae metadata y comentarios
2. **TikTok Collector**: Busca videos por hashtags, obtiene engagement (+ datos de Creative Center)
3. **Instagram Collector**: Rastrea hashtags, obtiene posts y stories
4. **Facebook Collector**: Monitorea páginas públicas y posts
5. **Google Trends Collector**: Extrae datos de búsquedas, interest over time, related queries

---

#### **4. Servicio de Procesamiento NLP**

**¿Qué es?**: Motor de análisis de lenguaje natural.

**¿Para qué sirve?**:
- Limpiar y normalizar texto (remover emojis, stopwords, URLs)
- Identificar temas automáticamente (Topic Modeling)
- Extraer entidades nombradas (NER): lugares, organizaciones, personas
- Analizar sentimiento: positivo, negativo, neutral
- Clasificar contenido según lineamientos
- Extraer keywords relevantes

**¿Por qué separarlo?**:
- Procesamiento NLP es computacionalmente intensivo
- Puede requerir GPU para modelos avanzados (BERT)
- Permite actualizar modelos sin afectar recolección
- Facilita procesar en lotes (batch processing)

**Técnicas que utilizará**:
- **Limpieza de texto**: Normalización, lemmatización
- **Topic Modeling**: BERTopic o LDA para identificar temas
- **NER (Named Entity Recognition)**: Extraer menciones de productos, lugares
- **Sentiment Analysis**: Detectar emociones y polaridad
- **Clasificación**: Asignar contenido a categorías del lineamiento

---

#### **5. Servicio de Analytics**

**¿Qué es?**: Motor de análisis y agregación de datos.

**¿Para qué sirve?**:
- Agregar datos por demografía (edad, género, ubicación)
- Calcular métricas: engagement, velocidad de crecimiento, trending
- Generar series temporales (evolución de temas)
- Identificar insights clave
- Producir respuestas en estructura JSON jerárquica

**¿Por qué separarlo?**:
- Análisis y recolección son procesos diferentes
- Permite consultas rápidas sin recargar recolección
- Facilita implementar cache y vistas materializadas
- Análisis puede ejecutarse en horarios de baja demanda

---

#### **6. Sistema de Colas (Message Queue)**

**¿Qué es?**: Orquestador de tareas asíncronas.

**¿Para qué sirve?**:
- Programar tareas de recolección periódicas
- Encolar procesamiento NLP de contenido nuevo
- Ejecutar análisis en background
- Reintentar tareas fallidas automáticamente
- Priorizar tareas urgentes

**¿Por qué necesitamos esto?**:
- Recolección puede tardar horas, no debe bloquear API
- Permite procesar miles de contenidos en paralelo
- Mejora resiliencia (tareas no se pierden si hay fallos)
- Facilita escalado horizontal (múltiples workers)

**Casos de uso**:
- Usuario solicita análisis → Cola crea tareas para cada collector → Collectors trabajan en paralelo → NLP procesa resultados → Analytics agrega datos → Usuario recibe notificación

---

#### **7. Base de Datos**

**¿Qué es?**: Almacenamiento persistente de todos los datos.

**¿Para qué sirve?**:
- Guardar lineamientos configurados
- Almacenar contenido recolectado de redes sociales
- Guardar temas identificados por NLP
- Almacenar demografía y métricas agregadas
- Guardar series temporales de tendencias

**¿Por qué PostgreSQL?**:
- Base de datos relacional robusta y madura
- Soporte para JSON (JSONB) para datos flexibles
- Extensión TimescaleDB para series temporales
- Excelente performance en agregaciones
- Full-text search nativo
- Gratuito y open source

---

#### **8. Cache (Redis)**

**¿Qué es?**: Capa de almacenamiento en memoria ultrarrápida.

**¿Para qué sirve?**:
- Cachear respuestas API frecuentes
- Implementar rate limiting
- Almacenar sesiones de usuarios
- Cache de vistas materializadas
- Actuar como message broker (para colas)

**¿Por qué Redis?**:
- Extremadamente rápido (operaciones en microsegundos)
- Reduce carga en PostgreSQL
- Mejora tiempo de respuesta de API
- Versátil (cache, pub/sub, rate limiting)

---

### 3.3 Flujo de Datos Completo

```
┌─────────────────────────────────────────────────────────────────┐
│                         USUARIO/CLIENTE                          │
│            (Aplicación web, dashboard, API client)               │
└────────────────────────────────┬────────────────────────────────┘
                                 │
                                 │ HTTP Request
                                 ▼
┌─────────────────────────────────────────────────────────────────┐
│                          API GATEWAY                             │
│          (Autenticación, Rate Limiting, Routing)                 │
└─────┬───────────────────────────────────────────────────┬───────┘
      │                                                     │
      │ Configurar lineamiento                            │ Consultar tendencias
      ▼                                                     ▼
┌──────────────────────┐                         ┌──────────────────────┐
│ Servicio Lineamientos│                         │  Servicio Analytics  │
│  - CRUD lineamientos │                         │  - Consultas         │
│  - Config keywords   │                         │  - Agregaciones      │
└──────────┬───────────┘                         │  - Exportación       │
           │                                     └──────────┬───────────┘
           │ Crear tarea recolección                       │
           ▼                                               │ Leer datos
┌─────────────────────────────────────────────┐           │
│        MESSAGE QUEUE (RabbitMQ/Celery)       │           │
│         - Cola de tareas                    │           │
│         - Scheduling                        │           │
│         - Retry logic                       │           │
└──┬────────┬────────┬────────┬───────────────┘          │
   │        │        │        │                           │
   │Task    │Task    │Task    │Task                       │
   ▼        ▼        ▼        ▼                           │
┌────────┐ ┌──────┐ ┌───────┐ ┌────────┐                │
│YouTube │ │TikTok│ │Insta  │ │Facebook│                │
│Collect.│ │Coll. │ │Coll.  │ │Collect.│                │
└───┬────┘ └───┬──┘ └───┬───┘ └───┬────┘                │
    │          │        │         │                       │
    │ API Call │        │         │                       │
    ▼          ▼        ▼         ▼                       │
┌────────┐ ┌──────┐ ┌───────┐ ┌────────┐                │
│YouTube │ │TikTok│ │Insta. │ │Facebook│                │
│  API   │ │ API  │ │  API  │ │  API   │                │
└───┬────┘ └───┬──┘ └───┬───┘ └───┬────┘                │
    │          │        │         │                       │
    │ Datos    │        │         │                       │
    └──────────┴────────┴─────────┘                       │
               │                                           │
               ▼                                           │
    ┌────────────────────┐                                │
    │  POSTGRESQL DB     │◄───────────────────────────────┘
    │  - contenido_      │
    │    recolectado     │
    └──────────┬─────────┘
               │
               │ Trigger: nuevo contenido
               ▼
    ┌────────────────────┐
    │  Servicio NLP      │
    │  - Topic Modeling  │
    │  - Sentiment       │
    │  - NER             │
    │  - Clasificación   │
    └──────────┬─────────┘
               │
               │ Temas identificados
               ▼
    ┌────────────────────┐
    │  POSTGRESQL DB     │
    │  - temas_          │
    │    identificados   │
    │  - demografia_tema │
    └──────────┬─────────┘
               │
               │ Agregación
               ▼
    ┌────────────────────┐
    │ Servicio Analytics │
    │ - Agrega por       │
    │   demografía       │
    │ - Calcula métricas │
    └──────────┬─────────┘
               │
               ▼
    ┌────────────────────┐
    │   REDIS CACHE      │
    │   - Resultados     │
    │     frecuentes     │
    └────────────────────┘
```

---

## 4. MODELO DE DATOS CONCEPTUAL

### 4.1 Entidades Principales

#### **Lineamientos**
Representa un área temática a analizar.
- ID, nombre, descripción
- Keywords a buscar
- Hashtags a rastrear
- Plataformas activas
- Frecuencia de recolección

#### **Contenido Recolectado**
Cada pieza de contenido de redes sociales.
- ID, lineamiento asociado
- Plataforma de origen
- ID externo (de la plataforma)
- Título, descripción, texto completo
- Autor y su ID
- Fecha de publicación
- Métricas: vistas, likes, comentarios, shares, guardados
- Ubicación geográfica: país, región, ciudad
- Metadata adicional (JSON flexible)

#### **Temas Identificados**
Temas extraídos por NLP.
- ID, lineamiento asociado
- Nombre del tema
- Descripción
- Keywords que lo definen
- Métricas agregadas: menciones totales, engagement
- Sentiment promedio
- Plataformas donde aparece

#### **Relación Contenido-Tema**
Relación muchos a muchos (un contenido puede tener varios temas).
- Contenido ID
- Tema ID
- Relevancia (0 a 1): qué tan relevante es el tema en ese contenido

#### **Demografía por Tema**
Agregación de métricas demográficas.
- Tema ID
- Plataforma
- Género (masculino, femenino, no_binario, desconocido)
- Rango de edad (13-17, 18-24, 25-34, 35-44, 45-54, 55-64, 65+)
- País, región, ciudad
- Métricas: interacciones totales, engagement promedio, sentiment

#### **Tendencias (Series Temporales)**
Evolución temporal de temas.
- Tema ID
- Plataforma
- Fecha y hora
- Menciones en esa hora
- Engagement en esa hora
- Usuarios únicos
- Velocidad de crecimiento (%)

---

### 4.2 Jerarquía de Respuesta JSON

Como solicitaste, la respuesta JSON sigue esta estructura:

```
Nivel 1: Plataforma Social (youtube, tiktok, instagram, facebook)
    └── Nivel 2: Ubicación Geográfica (país → región → ciudad)
        └── Nivel 3: Rango de Edad (18-24, 25-34, 35-44, etc.)
            └── Nivel 4: Género (masculino, femenino)
                └── Temas identificados con ejemplos de contenido
```

Esta jerarquía permite:
- Comparar plataformas fácilmente
- Identificar patrones geográficos
- Segmentar por demografía naturalmente
- Drill-down progresivo en los datos

---

## 5. ESTRATEGIA DE SEGMENTACIÓN DEMOGRÁFICA

### 5.1 Datos Directos vs Inferidos

#### **Datos Directos** (preferibles cuando disponibles):
- **Instagram**: Insights de cuenta de negocio (edad, género, ubicación de seguidores)
- **Facebook**: Insights de página (demografía completa de audiencia)
- **YouTube**: Analytics API (solo si somos dueños del canal)
- **TikTok**: Research API (ubicación, limitada demografía)

#### **Datos Inferidos** (cuando no hay directos):
- **Ubicación**:
  - Hashtags geográficos (#bogota, #colombia)
  - Menciones de lugares en texto
  - Idioma y variante regional
  - Zona horaria de publicación

- **Edad**:
  - Análisis del estilo de contenido
  - Lenguaje utilizado (jerga generacional)
  - Temas de interés
  - Modelos de ML entrenados con datos conocidos

- **Género**:
  - Análisis de nombre de usuario (cuando es obvio)
  - Estilo de escritura (con precaución, evitar estereotipos)
  - Auto-identificación en bio/descripción

### 5.2 Técnicas de Inferencia

#### **Análisis de Texto (NLP)**
- Detectar expresiones regionales: "parcero" (Colombia), "wey" (México), "boludo" (Argentina)
- Identificar menciones explícitas: "Vivo en Bogotá", "Soy de Medellín"

#### **Análisis de Metadata**
- Geolocalización de posts (cuando está disponible)
- Zona horaria de actividad
- Idioma declarado vs detectado

#### **Machine Learning**
- Entrenar clasificadores con datasets etiquetados
- Features: palabras utilizadas, emojis, temas de interés, estilo
- Importante: Usar con cuidado, ser transparente sobre precisión

### 5.3 Categorías Demográficas

#### **Rangos de Edad**:
- 13-17: Adolescentes
- 18-24: Jóvenes adultos / Gen Z
- 25-34: Millennials / Adultos jóvenes
- 35-44: Adultos establecidos
- 45-54: Adultos maduros
- 55-64: Pre-jubilados
- 65+: Adultos mayores

#### **Género**:
- Masculino
- Femenino
- No binario
- Desconocido/No especificado

#### **Ubicación**:
- País (nivel 1)
- Región/Estado/Departamento (nivel 2)
- Ciudad (nivel 3)

---

## 6. PROCESAMIENTO DE LENGUAJE NATURAL (NLP)

### 6.1 Objetivos del NLP

1. **Identificar temas**: ¿De qué habla este contenido?
2. **Extraer entidades**: ¿Qué productos, lugares, personas se mencionan?
3. **Analizar sentimiento**: ¿Es positivo, negativo o neutral?
4. **Clasificar**: ¿Pertenece a este lineamiento?

### 6.2 Técnicas a Utilizar

#### **Topic Modeling (Modelado de Temas)**
**¿Qué es?**: Técnica que identifica temas automáticamente en colecciones de texto.

**¿Para qué sirve?**:
- Descubrir temas sin necesidad de definirlos previamente
- Agrupar contenido similar
- Identificar tendencias emergentes

**Opciones**:
- **LDA (Latent Dirichlet Allocation)**: Clásico, rápido, requiere ajuste de parámetros
- **BERTopic**: Moderno, usa embeddings de BERT, más preciso, entiende contexto

**¿Por qué BERTopic?**:
- Entiende semántica (sabe que "invertir" y "inversión" son relacionados)
- Resultados más coherentes
- Menos ajuste manual necesario

#### **Named Entity Recognition (NER)**
**¿Qué es?**: Extracción de entidades nombradas en texto.

**¿Para qué sirve?**:
- Identificar productos mencionados ("Bancolombia", "Davivienda")
- Extraer lugares ("Bogotá", "Colombia", "Wall Street")
- Detectar personas ("Warren Buffett", "ministro de hacienda")

**¿Por qué usarlo?**:
- Enriquece el análisis con contexto específico
- Permite identificar menciones de marcas
- Facilita segmentación geográfica

#### **Sentiment Analysis (Análisis de Sentimiento)**
**¿Qué es?**: Determinar si un texto expresa emoción positiva, negativa o neutral.

**¿Para qué sirve?**:
- Entender percepción sobre temas financieros
- Identificar frustración o satisfacción
- Detectar controversias

**Escala**: -1 (muy negativo) a +1 (muy positivo)

**Ejemplo**:
- "¡Increíble cómo ahorré $1M con este método!" → Sentiment: +0.9
- "El crédito hipotecario es imposible de conseguir" → Sentiment: -0.7
- "Invertir en bolsa requiere educación" → Sentiment: 0.1 (neutral)

#### **Clasificación de Texto**
**¿Qué es?**: Asignar categorías predefinidas a texto.

**¿Para qué sirve?**:
- Verificar que contenido pertenece al lineamiento
- Categorizar por subtemas (ahorro, inversión, crédito)
- Filtrar contenido irrelevante

---

### 6.3 Procesamiento en Español

**Desafío**: La mayoría de modelos NLP están optimizados para inglés.

**Solución**:
- Usar modelos pre-entrenados en español
- spaCy: modelo `es_core_news_lg` (español)
- BERT: modelos `BETO` o `distilbert-base-multilingual`
- Entrenar/afinar modelos con datos del dominio financiero

---

## 7. MÉTRICAS Y KPIs

### 7.1 Métricas de Contenido Individual

- **Vistas/Reproducciones**: Alcance del contenido
- **Likes**: Aprobación directa
- **Comentarios**: Nivel de conversación generado
- **Shares/Compartidos**: Viralidad
- **Guardados**: Valor percibido (especialmente en Instagram)
- **Engagement Rate**: (Likes + Comentarios + Shares) / Vistas * 100

### 7.2 Métricas de Temas

- **Menciones**: Cuántas veces se habla del tema
- **Engagement Total**: Suma de todas las interacciones
- **Engagement Promedio**: Engagement total / Menciones
- **Sentiment Promedio**: Promedio de sentiment de todos los contenidos
- **Velocidad de Crecimiento**: % de aumento en menciones (últimas 24h vs 24h anteriores)
- **Trending**: Booleano indicando si está en crecimiento acelerado

### 7.3 Métricas Demográficas

- **Participación por Segmento**: % de interacciones de cada demografía
- **Engagement por Segmento**: Engagement promedio por grupo
- **Sentiment por Segmento**: Cómo varía la percepción por demografía

### 7.4 Definición de "Trending"

Un tema es **trending** cuando:
1. Velocidad de crecimiento > 15% en últimas 24 horas
2. Mínimo 50 menciones en el período
3. Engagement rate > promedio de la plataforma

---

## 8. STACK TECNOLÓGICO

### 8.1 Backend

#### **Python 3.11+**
**¿Por qué?**:
- Ecosistema rico en librerías de data science y NLP
- Excelente para APIs (FastAPI)
- Comunidad grande, fácil encontrar soluciones
- Legible y mantenible

#### **FastAPI**
**¿Qué es?**: Framework web moderno para construir APIs.
**¿Por qué?**:
- Extremadamente rápido (basado en Starlette/ASGI)
- Async/await nativo (importante para I/O)
- Documentación automática (Swagger UI)
- Validación automática con Pydantic
- Tipado estático (type hints)

---

### 8.2 Procesamiento de Datos

#### **Pandas**
**¿Para qué?**: Manipulación y análisis de datos tabulares.
**¿Por qué?**: Estándar de facto en Python para data wrangling.

#### **spaCy**
**¿Para qué?**: Procesamiento NLP en español.
**¿Por qué?**: Rápido, production-ready, modelos en español.

#### **Transformers (Hugging Face)**
**¿Para qué?**: Modelos BERT para embeddings y clasificación.
**¿Por qué?**: Estado del arte en NLP, modelos pre-entrenados disponibles.

#### **BERTopic**
**¿Para qué?**: Topic modeling avanzado.
**¿Por qué?**: Mejores resultados que LDA clásico, menos tuning.

---

### 8.3 APIs de Redes Sociales

#### **google-api-python-client**
Cliente oficial de Google para YouTube Data API.

#### **TikTokApi** (o TikTok Research API)
Acceso a TikTok. No oficial para MVP, oficial para producción.

#### **facebook-sdk**
Cliente para Instagram y Facebook Graph API.

#### **pytrends**
Biblioteca Python no oficial para acceso programático a Google Trends. Permite extraer interest over time, related queries, trending searches.

#### **Opcional: Serpapi**
Servicio de pago que proporciona API oficial para Google Trends con mayor estabilidad y límites más altos.

---

### 8.4 Infraestructura

#### **PostgreSQL 15+**
**¿Por qué?**:
- Robusto, maduro, confiable
- Excelente para análisis (agregaciones, joins)
- Soporte JSON (JSONB) para flexibilidad
- Extensión TimescaleDB para series temporales
- Gratuito y open source

#### **Redis**
**¿Por qué?**:
- Cache ultrarrápido (reduce latencia)
- Message broker para Celery
- Rate limiting
- Almacenamiento de sesiones

#### **Celery + RabbitMQ**
**¿Para qué?**: Task queue para procesamiento asíncrono.
**¿Por qué?**:
- Permite tareas en background
- Reintentos automáticos
- Scheduling (cron jobs)
- Escalabilidad horizontal (múltiples workers)

#### **Docker + Docker Compose**
**¿Para qué?**: Containerización y orquestación local.
**¿Por qué?**:
- Desarrollo consistente entre entornos
- Fácil deployment
- Aislamiento de servicios
- Reproducibilidad

---

## 9. CONSIDERACIONES DE ESCALABILIDAD

### 9.1 Límites de APIs

| API | Límite Gratuito | Estrategia |
|-----|-----------------|------------|
| YouTube Data API v3 | 10,000 unidades/día | Múltiples API keys, cache agresivo |
| TikTok Research | Por aprobación | Priorizar contenido popular |
| TikTok Creative Center | Sin límite oficial | Scraping moderado con delays |
| Instagram Graph API | 200 req/hora/usuario | Múltiples cuentas autorizadas |
| Facebook Graph API | 200 req/hora/usuario | Compartir con Instagram, cache |
| Google Trends | Sin límite oficial | Delays entre requests (pytrends) |

### 9.2 Volumen de Datos Estimado

**Para 1 lineamiento (ej: finanzas)**:
- 4 plataformas de redes sociales (YouTube, TikTok, Instagram, Facebook)
- 2 fuentes complementarias (TikTok Creative Center, Google Trends)
- ~1,000 contenidos/día por plataforma social
- ~50-100 keywords monitoreados en Google Trends
- ~4,000 contenidos/día totales de redes sociales
- ~120,000 contenidos/mes + datos de búsquedas y tendencias

**Almacenamiento**:
- Texto + metadata: ~5GB/mes
- Crecimiento anual: ~60GB
- Con 10 lineamientos: ~600GB/año

**Estrategia**:
- Particionamiento de tablas por fecha
- Archivado de datos >6 meses (S3, Google Cloud Storage)
- Compresión automática (PostgreSQL TOAST)

### 9.3 Escalamiento Horizontal

**Collectors**: Múltiples instancias por plataforma si volumen aumenta
**Workers NLP**: Múltiples workers Celery con GPU
**Cache**: Redis Cluster para mayor throughput
**Base de datos**: Read replicas para consultas, write master

---

## 10. SEGURIDAD Y PRIVACIDAD

### 10.1 Manejo de API Keys

- Almacenar en variables de entorno (NO en código)
- Usar servicios de secrets management (AWS Secrets Manager, Vault)
- Rotación periódica de keys
- Rate limiting para prevenir abuso

### 10.2 Datos de Usuarios

- Solo recolectar datos públicos
- Anonimizar información personal inferida
- Cumplir con GDPR y regulaciones locales
- No almacenar datos sensibles sin encriptar

### 10.3 Autenticación API

- JWT tokens para autenticación
- API keys para clientes
- Rate limiting por usuario
- Logging de todas las peticiones

---

## 11. PLAN DE IMPLEMENTACIÓN POR FASES

### **Fase 1: MVP (4-6 semanas)**
**Objetivo**: Demostrar viabilidad del concepto.

**Componentes**:
- Setup de infraestructura (Docker, PostgreSQL, Redis)
- API Gateway básico
- Servicio de Lineamientos (CRUD simple)
- 2 Collectors funcionales:
  - YouTube Collector (el más documentado)
  - Google Trends Collector (pytrends - GRATUITO)
- NLP básico (limpieza + topic modeling simple con LDA)
- API de consulta básica (sin jerarquía completa)
- Frontend mínimo (dashboard simple)
- **Validación cruzada**: Comparar temas de YouTube con búsquedas de Google Trends

**Entregables**:
- Sistema funcional con YouTube + Google Trends
- Puede identificar 3-5 temas en un lineamiento
- Dashboard muestra temas, ejemplos Y validación con búsquedas
- Primer insight de "content gap" (alta búsqueda, bajo contenido)

---

### **Fase 2: Expansión Multiplataforma (4-6 semanas)**
**Objetivo**: Agregar todas las plataformas principales.

**Componentes**:
- Collectors adicionales:
  - TikTok Collector (Research API + Creative Center)
  - Instagram Collector
  - Facebook Collector
- Message queue (Celery + RabbitMQ)
- NLP mejorado (BERTopic, sentiment analysis)
- Inferencia demográfica básica
- Estructura JSON jerárquica completa
- **Insights de correlación**: Validación cross-platform con Google Trends

**Entregables**:
- Sistema multiplatforma completo (4 redes sociales + Google Trends)
- Segmentación demográfica funcional
- API retorna estructura jerárquica (plataforma → ubicación → edad → género)
- Insights automáticos: content gaps, trend validation, predicción temprana

---

### **Fase 3: Optimización y Features Avanzadas (4-6 semanas)**
**Objetivo**: Producción-ready, análisis avanzado.

**Componentes**:
- ML para inferencia demográfica avanzada
- Series temporales y detección de trending
- Alertas en tiempo real
- Exportación de reportes (PDF, Excel)
- Dashboard avanzado con visualizaciones
- Optimización de performance (cache, índices)
- Testing exhaustivo (unit, integration, e2e)
- Documentación completa

**Entregables**:
- Sistema listo para producción
- Monitoreo y alertas
- Documentación de usuario y desarrollador

---

### **Fase 4: Escalamiento (Continuo)**
**Objetivo**: Manejar múltiples clientes y alto volumen.

**Componentes**:
- Multi-tenancy (múltiples organizaciones)
- Escalamiento horizontal automatizado
- Migración a Kubernetes (si es necesario)
- CDN para assets
- Sistema de billing/facturación

---

## 12. RIESGOS Y MITIGACIONES

### 12.1 Riesgos Técnicos

| Riesgo | Impacto | Mitigación |
|--------|---------|------------|
| APIs cambian o se deprecan | Alto | Abstraer collectors, fácil reemplazar implementación |
| Rate limits excedidos | Medio | Múltiples keys, cache agresivo, priorización |
| Volumen de datos crece más de lo esperado | Medio | Particionamiento, archivado automático, cloud storage |
| NLP no identifica temas precisos | Alto | Usar BERTopic + ajustar parámetros, entrenar modelos custom |
| Performance de consultas lenta | Medio | Vistas materializadas, cache Redis, índices optimizados |

### 12.2 Riesgos de Negocio

| Riesgo | Impacto | Mitigación |
|--------|---------|------------|
| APIs de pago son muy costosas | Alto | Comenzar con tiers gratuitos, evaluar ROI antes de pagar |
| Competencia con herramientas existentes | Medio | Diferenciación: enfoque en LATAM, demografía específica |
| Adopción lenta de usuarios | Medio | MVP rápido, feedback temprano, iterar |

---

## 13. MÉTRICAS DE ÉXITO DEL PROYECTO

### 13.1 Métricas Técnicas
- **Cobertura**: >80% de contenido relevante recolectado
- **Precisión NLP**: >70% de temas identificados correctamente
- **Performance**: API responde en <2 segundos para consultas complejas
- **Uptime**: >99% disponibilidad
- **Latencia de recolección**: Contenido nuevo detectado en <6 horas

### 13.2 Métricas de Producto
- **Usuarios activos**: X usuarios mensuales
- **Lineamientos activos**: X lineamientos configurados
- **Consultas API**: X consultas/día
- **Satisfacción**: NPS >50

---

## 14. PRÓXIMOS PASOS INMEDIATOS

### Antes de comenzar implementación:

1. **Obtener acceso a APIs y herramientas**:
   - [ ] Google Cloud Console → Activar YouTube Data API v3
   - [ ] Meta for Developers → Crear app, obtener tokens para Graph API
   - [ ] TikTok for Developers → Solicitar acceso a Research API (puede tardar semanas)
   - [ ] **Google Trends → Instalar pytrends** (`pip install pytrends`)
   - [ ] **TikTok Creative Center → Crear cuenta** (https://ads.tiktok.com/business/creativecenter)
   - [ ] Probar cada API con requests mínimos

2. **Setup de desarrollo**:
   - [ ] Instalar Docker y Docker Compose
   - [ ] Configurar repositorio Git
   - [ ] Definir estructura de proyecto
   - [ ] Setup de entorno virtual Python
   - [ ] Instalar dependencias iniciales (pytrends, google-api-python-client, etc.)

3. **Validaciones técnicas**:
   - [ ] Hacer peticiones de prueba a cada API
   - [ ] **Probar pytrends**: Extraer interest over time para un keyword de prueba
   - [ ] **Validar TikTok Creative Center**: Acceso a dashboard de hashtags trending
   - [ ] Verificar rate limits reales
   - [ ] Probar modelos NLP en español con datos de ejemplo
   - [ ] Estimar costos reales de infraestructura

4. **Documentación**:
   - [ ] Crear documento de implementación técnica (siguiente paso)
   - [ ] Definir estructura de código y convenciones
   - [ ] Setup de herramientas de desarrollo (linters, formatters)

---

## 15. CONCLUSIÓN

Este sistema de análisis de tendencias en redes sociales es **viable técnicamente** y **valioso comercialmente**.

**Factores de éxito clave**:
1. Arquitectura de microservicios permite flexibilidad y escalabilidad
2. **6 fuentes de datos complementarias**: 4 redes sociales + Google Trends + TikTok Creative Center
3. **Google Trends como validador**: Captura intención de búsqueda, no solo consumo pasivo
4. APIs oficiales proporcionan datos confiables
5. NLP moderno (BERTopic, BERT) mejora precisión de identificación de temas
6. Estructura JSON jerárquica facilita consultas segmentadas
7. Enfoque en LATAM y español nos diferencia

**Diferenciales únicos**:
- **Validación cross-platform**: Tendencias confirmadas entre redes sociales Y búsquedas de Google
- **Detección de content gaps**: Alta búsqueda + Bajo contenido = Oportunidades
- **Predicción temprana**: Google Trends detecta tendencias 2-4 semanas antes
- Segmentación demográfica detallada (edad, género, ubicación)
- Multi-plataforma (visión holística de 4 redes sociales)
- Análisis en español optimizado
- Identificación automática de temas (no solo keywords)
- Series temporales para detectar tendencias emergentes

**Siguiente paso**: Crear documento de implementación técnica con código, configuraciones y guías paso a paso.

---

**¿Listo para proceder con el documento de implementación técnica?**
