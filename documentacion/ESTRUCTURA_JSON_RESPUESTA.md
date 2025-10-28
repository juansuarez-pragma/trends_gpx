# ESTRUCTURA JSON DE RESPUESTA API

## Jerarquía de Agrupamiento

La respuesta de la API sigue una estructura jerárquica de 4 niveles:

```
Nivel 1: PLATAFORMA SOCIAL
    └── Nivel 2: UBICACIÓN GEOGRÁFICA (País → Región/Ciudad)
        └── Nivel 3: RANGO DE EDAD
            └── Nivel 4: GÉNERO
                └── Datos: Temas identificados + Métricas + Ejemplos
```

---

## Endpoint Principal

```
GET /api/analisis/tendencias?lineamiento={lineamiento}
```

### Parámetros de filtrado (opcionales):
- `plataforma`: youtube | tiktok | instagram | facebook
- `pais`: Código o nombre del país
- `region`: Nombre de región/ciudad
- `edad_min`: Edad mínima (número)
- `edad_max`: Edad máxima (número)
- `genero`: masculino | femenino
- `fecha_inicio`: Fecha inicio análisis (ISO 8601)
- `fecha_fin`: Fecha fin análisis (ISO 8601)
- `min_menciones`: Mínimo de menciones para incluir tema
- `trending_only`: true | false

---

## Estructura Completa de Respuesta

```json
{
  "lineamiento": {
    "id": 1,
    "nombre": "finanzas",
    "descripcion": "Análisis de tendencias en finanzas personales",
    "periodo_analisis": {
      "fecha_inicio": "2025-10-01T00:00:00Z",
      "fecha_fin": "2025-10-27T23:59:59Z",
      "dias_analizados": 27
    }
  },

  "metricas_globales": {
    "total_contenidos_analizados": 15420,
    "total_interacciones": 2450000,
    "engagement_promedio": 158.9,
    "sentiment_general": 0.62,
    "temas_identificados": 12
  },

  "plataformas": {

    "{PLATAFORMA}": {
      "metricas_plataforma": {
        "total_contenidos": 3500,
        "total_vistas": 1200000,
        "total_likes": 85000,
        "total_comentarios": 45000,
        "total_compartidos": 25000,
        "engagement_rate": 10.8
      },

      "ubicaciones": {

        "{PAIS}": {
          "metricas_pais": {
            "total_contenidos": 1250,
            "total_interacciones": 420000,
            "engagement_promedio": 336.0,
            "sentiment_promedio": 0.68
          },

          "regiones": {

            "{REGION/CIUDAD}": {
              "metricas_region": {
                "total_contenidos": 580,
                "total_interacciones": 195000,
                "engagement_promedio": 336.2
              },

              "rangos_edad": {

                "{RANGO_EDAD}": {
                  "metricas_edad": {
                    "total_contenidos": 120,
                    "total_interacciones": 42000,
                    "engagement_promedio": 350.0,
                    "porcentaje_participacion": 20.7
                  },

                  "generos": {

                    "{GENERO}": {
                      "metricas_genero": {
                        "total_interacciones": 25200,
                        "engagement_promedio": 360.0,
                        "porcentaje_participacion": 60.0,
                        "sentiment_promedio": 0.72
                      },

                      "temas": [
                        {
                          "id": 101,
                          "nombre": "Inversión en bolsa de valores",
                          "menciones": 45,
                          "engagement": 16200,
                          "sentiment": 0.75,
                          "keywords": ["bolsa", "invertir", "acciones", "trading"],
                          "hashtags": ["#inversionescolombia", "#bolsadevalores"],
                          "velocidad_crecimiento": 15.3,
                          "trending": true,

                          "ejemplos_contenido": [
                            {
                              "id": "vid_yt_001",
                              "titulo": "Cómo invertir en la bolsa de valores",
                              "url": "https://youtube.com/watch?v=abc123",
                              "autor": "Finanzas Para Jóvenes",
                              "fecha_publicacion": "2025-10-25T14:30:00Z",
                              "vistas": 12500,
                              "likes": 980,
                              "comentarios": 145,
                              "compartidos": 67,
                              "sentiment": 0.8,
                              "engagement_rate": 9.5
                            }
                          ]
                        }
                      ]
                    }
                  }
                }
              }
            }
          }
        }
      }
    }
  },

  "insights_clave": [
    {
      "tipo": "trending_topic",
      "descripcion": "El tema 'Plan de ahorros' crece 28% en TikTok Bogotá (18-24, masculino)",
      "relevancia": "alta",
      "plataforma": "tiktok",
      "tema_id": 201
    }
  ],

  "metadata": {
    "fecha_generacion": "2025-10-27T19:45:00Z",
    "tiempo_procesamiento_ms": 3450,
    "version_api": "1.0.0"
  }
}
```

---

## Ejemplo Real: YouTube en Colombia

```json
{
  "plataformas": {
    "youtube": {
      "metricas_plataforma": {
        "total_videos": 3500,
        "total_vistas": 1200000,
        "total_comentarios": 45000,
        "total_likes": 85000,
        "engagement_rate": 10.8
      },
      "ubicaciones": {
        "colombia": {
          "metricas_pais": {
            "total_contenidos": 1250,
            "total_interacciones": 420000,
            "engagement_promedio": 336.0,
            "sentiment_promedio": 0.68
          },
          "regiones": {
            "bogota": {
              "metricas_region": {
                "total_contenidos": 580,
                "total_interacciones": 195000,
                "engagement_promedio": 336.2
              },
              "rangos_edad": {
                "18-24": {
                  "metricas_edad": {
                    "total_contenidos": 120,
                    "total_interacciones": 42000,
                    "engagement_promedio": 350.0,
                    "porcentaje_participacion": 20.7
                  },
                  "generos": {
                    "masculino": {
                      "metricas_genero": {
                        "total_interacciones": 25200,
                        "engagement_promedio": 360.0,
                        "porcentaje_participacion": 60.0,
                        "sentiment_promedio": 0.72
                      },
                      "temas": [
                        {
                          "id": 101,
                          "nombre": "Inversión en bolsa de valores",
                          "menciones": 45,
                          "engagement": 16200,
                          "sentiment": 0.75,
                          "keywords": ["bolsa", "invertir", "acciones", "trading", "colombiana"],
                          "hashtags": ["#inversionescolombia", "#bolsadevalores", "#trading"],
                          "velocidad_crecimiento": 15.3,
                          "trending": true,
                          "ejemplos_contenido": [
                            {
                              "id": "vid_yt_001",
                              "titulo": "Cómo invertir en la bolsa de valores de Colombia desde cero",
                              "url": "https://youtube.com/watch?v=abc123",
                              "autor": "Finanzas Para Jóvenes",
                              "fecha_publicacion": "2025-10-25T14:30:00Z",
                              "vistas": 12500,
                              "likes": 980,
                              "comentarios": 145,
                              "compartidos": 67,
                              "duracion_segundos": 720,
                              "sentiment": 0.8,
                              "engagement_rate": 9.5
                            }
                          ]
                        },
                        {
                          "id": 102,
                          "nombre": "Criptomonedas y Bitcoin",
                          "menciones": 38,
                          "engagement": 9000,
                          "sentiment": 0.55,
                          "keywords": ["bitcoin", "criptomonedas", "crypto", "ethereum"],
                          "hashtags": ["#bitcoin", "#crypto", "#criptomonedas"],
                          "velocidad_crecimiento": 22.5,
                          "trending": true,
                          "ejemplos_contenido": []
                        }
                      ]
                    },
                    "femenino": {
                      "metricas_genero": {
                        "total_interacciones": 16800,
                        "engagement_promedio": 340.0,
                        "porcentaje_participacion": 40.0,
                        "sentiment_promedio": 0.70
                      },
                      "temas": [
                        {
                          "id": 103,
                          "nombre": "Ahorro e independencia financiera",
                          "menciones": 52,
                          "engagement": 12600,
                          "sentiment": 0.78,
                          "keywords": ["ahorro", "independencia financiera", "presupuesto"],
                          "hashtags": ["#ahorro", "#independenciafinanciera"],
                          "velocidad_crecimiento": 18.2,
                          "trending": true,
                          "ejemplos_contenido": []
                        }
                      ]
                    }
                  }
                },
                "25-34": {
                  "metricas_edad": {
                    "total_contenidos": 235,
                    "total_interacciones": 79500,
                    "engagement_promedio": 338.3,
                    "porcentaje_participacion": 40.8
                  },
                  "generos": {
                    "masculino": {
                      "metricas_genero": {},
                      "temas": []
                    },
                    "femenino": {
                      "metricas_genero": {},
                      "temas": []
                    }
                  }
                }
              }
            }
          }
        }
      }
    }
  }
}
```

---

## Catálogos de Valores

### Plataformas Soportadas
- `youtube`
- `tiktok`
- `instagram`
- `facebook`

### Rangos de Edad
- `13-17`: Adolescentes
- `18-24`: Jóvenes adultos / Gen Z
- `25-34`: Millennials
- `35-44`: Adultos establecidos
- `45-54`: Adultos maduros
- `55-64`: Pre-jubilados
- `65+`: Adultos mayores

### Géneros
- `masculino`
- `femenino`
- `no_binario`
- `desconocido`

### Tipos de Insights
- `trending_topic`: Tema en crecimiento acelerado
- `demographic_insight`: Patrón demográfico interesante
- `platform_comparison`: Comparación entre plataformas
- `sentiment_alert`: Alerta de sentiment inusual
- `geographic_trend`: Tendencia geográfica

---

## Campos y Definiciones

### Métricas de Plataforma
- `total_contenidos`: Número de videos/posts recolectados
- `total_vistas`: Suma de vistas (YouTube, TikTok)
- `total_likes`: Suma de likes
- `total_comentarios`: Suma de comentarios
- `total_compartidos`: Suma de shares
- `engagement_rate`: (Likes + Comentarios + Shares) / Vistas * 100

### Métricas Demográficas
- `engagement_promedio`: Promedio de interacciones por contenido
- `porcentaje_participacion`: % del total de interacciones de ese nivel
- `sentiment_promedio`: Rango de -1 (negativo) a +1 (positivo)

### Campos de Tema
- `menciones`: Número de contenidos que hablan de este tema
- `engagement`: Suma total de interacciones
- `sentiment`: Sentiment promedio del tema (-1 a +1)
- `keywords`: Palabras clave que definen el tema
- `hashtags`: Hashtags más usados relacionados
- `velocidad_crecimiento`: % de crecimiento vs período anterior
- `trending`: true si cumple criterios de trending (crecimiento > 15%, min 50 menciones)

### Ejemplo de Contenido
- `id`: Identificador único del contenido
- `titulo`: Título del video/post
- `url`: URL del contenido original
- `autor`: Nombre del creador
- `fecha_publicacion`: Fecha ISO 8601
- `vistas`: Número de vistas (si aplica)
- `likes`: Número de likes
- `comentarios`: Número de comentarios
- `compartidos`: Número de shares
- `sentiment`: Sentiment del contenido (-1 a +1)
- `engagement_rate`: Tasa de engagement individual

---

## Ejemplos de Queries

### Query 1: Todas las plataformas, lineamiento finanzas
```
GET /api/analisis/tendencias?lineamiento=finanzas
```

### Query 2: Solo YouTube en Colombia
```
GET /api/analisis/tendencias?lineamiento=finanzas&plataforma=youtube&pais=colombia
```

### Query 3: Jóvenes 18-24 masculinos en Bogotá, todas plataformas
```
GET /api/analisis/tendencias?lineamiento=finanzas&pais=colombia&region=bogota&edad_min=18&edad_max=24&genero=masculino
```

### Query 4: Solo temas trending en TikTok
```
GET /api/analisis/tendencias?lineamiento=finanzas&plataforma=tiktok&trending_only=true
```

### Query 5: Última semana, mínimo 100 menciones
```
GET /api/analisis/tendencias?lineamiento=finanzas&fecha_inicio=2025-10-20&fecha_fin=2025-10-27&min_menciones=100
```

---

## Ventajas de Esta Estructura

### 1. Jerarquía Lógica
Permite drill-down natural:
1. ¿Qué plataforma es más relevante?
2. ¿En qué países/regiones?
3. ¿Qué grupos de edad?
4. ¿Diferencias por género?

### 2. Fácil Comparación
- Entre plataformas (mismo país, edad, género)
- Entre regiones (misma plataforma, edad, género)
- Entre grupos demográficos

### 3. Filtrado Eficiente
Cada nivel puede filtrarse independientemente sin reestructurar la query.

### 4. Escalable
Fácil agregar nuevas plataformas o categorías demográficas sin romper estructura.

### 5. Optimizado para Visualización
Estructura perfecta para gráficos:
- Barras comparativas por plataforma
- Mapas de calor geográficos
- Pirámides demográficas
- Series temporales

---

## Próximos Pasos

Una vez aprobada esta estructura, procederemos con:
1. Diseño del esquema de base de datos para generar esta estructura
2. Consultas SQL optimizadas
3. Implementación de endpoints FastAPI
4. Documentación técnica de implementación
