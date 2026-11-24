# 🎣 Pesca Cádiz

Predicción semanal de jornadas de pesca desde playa para **lubina, dorada, sargo y herrera** en 21 playas y espigones de la provincia de Cádiz (Cádiz, San Fernando, Puerto Real, Chiclana y El Puerto de Santa María).

App web instalable (PWA), sin dependencias ni paso de compilación: todo está en `index.html`.

## Qué incluye

- Índice de pesca hora a hora por especie con semáforo (verde ≥ 80, azul 65–79, amarillo 50–64, rojo < 50, ⚠ veto de seguridad).
- Previsión a 7 días, gráfica de 24 h con la mejor ventana de 3 horas y desglose de cada componente.
- Tabla horaria unificada: índice por especie, viento, ola, presión, marea y luz/solunar.
- Mapa con vista dividida (esquema del litoral + tabla horaria) y capas Batimetría, Satélite y Carta.
- Buscador de playas, favoritas y comparador (ranking automático y comparación en paralelo de hasta 3).
- Enlaces a Apple Maps, Google Maps y Waze.
- Diario de pesca privado con estadísticas (se guarda en el navegador, `localStorage`).

## Algoritmo

`S = 0,35·M + 0,25·V + 0,15·O + 0,10·P + 0,10·T + 0,05·L`

| Componente | Peso | Entradas |
|---|---|---|
| M · Marea | 35 % | distancia a pleamar/bajamar y coeficiente |
| V · Viento | 25 % | velocidad, dirección respecto a la orientación de la playa y rachas |
| O · Oleaje y fondo | 15 % | altura y periodo de ola, tipo de fondo, corriente |
| P · Presión | 10 % | tendencia en 6 h y nivel absoluto |
| T · Temperatura | 10 % | agua frente al óptimo de la especie |
| L · Luz y luna | 5 % | amanecer/atardecer, noche y solunar |

Veto: viento > 40 km/h, ola > 2 m o aviso de AEMET → «No recomendable».
La lógica está en la función `calc()` de `index.html`. Los parámetros por especie están en `SPC` y las playas en `S`.

## Datos

- **Con conexión** (por ejemplo, publicada en GitHub Pages): la app pide viento, rachas, presión, oleaje, periodo y temperatura del agua a [Open-Meteo](https://open-meteo.com) y arriba a la derecha aparece «Open-Meteo en vivo».
- **Sin conexión o si falla la petición**: usa datos simulados y lo indica como «Datos simulados (demo)».
- **Mareas y coeficiente**: estimación armónica, no oficial. Pendiente integrar la API del IHM.
- **Avisos**: el aviso AEMET está simulado. Pendiente integrar AEMET OpenData (requiere clave de API).
- **Batimetría**: el mapa es un esquema. Pendiente integrar EMODnet y cartas náuticas.
- Coordenadas y orientaciones de playa **aproximadas**: revísalas en el array `S`.

## Ejecutar en local

```bash
python3 -m http.server 8000
# abre http://localhost:8000
```

## Publicar en GitHub Pages

1. Sube estos archivos a la raíz de un repositorio.
2. En **Settings → Pages**, elige **Deploy from a branch**, rama `main` y carpeta `/ (root)`.
3. En unos minutos tendrás la app en `https://TU_USUARIO.github.io/NOMBRE_REPO/`.

## Instalar en el iPhone

Abre la URL en Safari → **Compartir → Añadir a pantalla de inicio**.

## Próximos pasos

- Mareas reales y coeficiente oficial del IHM.
- Avisos costeros de AEMET para el veto automático.
- Batimetría EMODnet y mapa con teselas (por ejemplo, Leaflet).
- Recalibrar los pesos con el diario (regresión logística tras 30–50 jornadas).

## Aviso

Es una ayuda para planificar, no una garantía de capturas ni de seguridad. Comprueba siempre los avisos oficiales, el estado del mar y el acceso antes de salir.
