# Tablero de Telecomunicaciones — OSIPTEL (PUNKU)

Tablero dinámico e interactivo de indicadores de telecomunicaciones del Perú, con **mapa coroplético** por nivel territorial, penetración por cada 100 habitantes, concentración de mercado y ranking de empresas operadoras. Elaboración: **Good Guys Insight**.

## Demo

El tablero es una única página autocontenida (`index.html`, incluye la librería Leaflet y los datos; funciona sin conexión). Para publicarla:

1. En este repositorio: **Settings → Pages → Build and deployment → Source: Deploy from a branch**, rama `main`, carpeta `/ (root)`.
2. La página quedará disponible en `https://goodguysinsight.github.io/tablero_osiptel/`.

También puede abrirse descargando `index.html` y abriéndolo en el navegador.

## Funcionalidades

Tres pestañas:

- **Internet fijo · mapa distrital**
  - **Tres niveles territoriales:** departamental, provincial y distrital, con **filtros en cascada** (Departamento → Provincia → Distrito).
  - **Indicadores del mapa:** conexiones, **conexiones por cada 100 habitantes**, variación interanual, **% de fibra óptica**, **N° de empresas** y **concentración (IHH)**.
  - **Filtros:** tecnología (fibra óptica, cablemódem, xDSL, satelital, Wimax, otras), segmento (residencial / comercial), trimestre (mar‑2023 a mar‑2026) y empresa.
  - **Panel por zona:** conexiones, penetración, población 2025, variación interanual, % fibra, N° de empresas, empresa líder y su cuota, IHH, y **serie trimestral** de conexiones.
  - **Principales empresas:** Top 10 de operadores de la zona con su número de conexiones y cuota.
- **Servicios · mapa departamental**
  - **Servicios:** líneas móviles, internet fijo, TV de paga, telefonía fija, teléfonos públicos y reclamos presentados.
  - **Indicadores:** valor, **por cada 100 habitantes**, % del total nacional y variación interanual.
  - **Filtros:** categoría propia de cada servicio (modalidad contractual, tecnología, medio de acceso, tipo de línea o servicio involucrado), mes y empresa.
  - Panel con serie mensual del departamento y Top 10 de empresas.
- **Tabla de datos:** todas las zonas con los filtros vigentes del mapa, ordenable por columna y descargable en CSV.

**Clic en una zona** la fija en el panel y en el ranking; el botón **«Ver todo»** regresa a la vista nacional. Diseño **responsive**: en escritorio (más de 1,024 px) los paneles flotan sobre el mapa; en tablet pasan a dos columnas bajo el mapa y en celular a una sola columna, con filtros a ancho completo. Incluye modo claro y oscuro.

**Formato numérico:** miles con coma (,) y decimales con punto (.) en todo el tablero y en el CSV exportado (p. ej., 4,135,995 conexiones; 12.6 por cada 100 habitantes), independiente del idioma del navegador.

## Fuente de datos

- **Indicadores de telecomunicaciones:** OSIPTEL — plataforma **PUNKU**, datasets de descarga (oct‑2026):
  - 2.3 Conexiones de internet fijo por distrito (trimestral, mar‑2023 a mar‑2026).
  - 1.1 Líneas móviles en servicio · 2.1 Conexiones de internet fijo · 3.1 Conexiones de TV de paga · 4.1 Líneas fijas de abonado en servicio · 6.1 Líneas de telefonía de uso público (desde 2018).
  - 10.1 Reclamos presentados (desde ene‑2023, primer año con detalle departamental).
- **Población:** INEI — **Censos Nacionales 2025** (XIII de Población, VIII de Vivienda y IV de Comunidades Indígenas), población censada por distrito.
- **Cartografía (límites departamentales, provinciales y distritales):** Registro Nacional de Límites (**RENLIM**) — Secretaría de Demarcación y Organización Territorial (**SDOT – PCM**), vigencia 2023 (referenciales).

## Datos

`datos_tablero_osiptel.xlsx` — base del tablero:

| Hoja | Contenido |
|---|---|
| `Leeme` | Descripción de hojas, fuentes y notas metodológicas |
| `IF_distrital_resumen` | Una fila por distrito (1,891): ubigeo, población 2025, conexiones mar‑2026 y mar‑2025, conexiones de fibra, N° de empresas, IHH, empresa líder y cuota; variación interanual, conexiones por 100 hab. y % fibra como **fórmulas**; fila de total nacional |
| `IF_distrital_detalle` | Conexiones de internet fijo por trimestre, ubigeo, empresa, tecnología y segmento (~172,000 filas) |
| `Departamental` | Series por servicio, mes, departamento, empresa y categoría (~144,000 filas) |
| `Poblacion_2025` | Población censada 2025 por distrito, ajustada a la cartografía RENLIM 2023 |

## Metodología

- **Ubigeo:** el dataset 2.3 no trae ubigeo; se asigna por nombre normalizado (departamento, provincia y distrito, sin tildes ni signos) contra la cartografía RENLIM. Coinciden 1,885 de 1,889 combinaciones; las 4 restantes, distritos de creación reciente, se asignan manualmente: Huaya → Hualla (051010), Huayllo → Ihuayllo (030407), Laraos → San Pedro de Laraos (150712) y Santa Rosa de Loreto → Yavarí (160403). Representan menos del 0.001 % de las conexiones.
- **Población:** se usa la del Censo 2025 por ubigeo. La de **Santa Rosa de Loreto** (160405) se suma a Yavarí y la de **Sangani** (120307) a Pichanaqui, porque la cartografía 2023 y OSIPTEL aún no los registran como distritos. La asignación de Sangani a Pichanaqui es una inferencia (la caída de población de Pichanaqui) y está pendiente de verificar con la ley de creación.
- **Indicadores:**
  - Conexiones (o líneas) por cada 100 habitantes = valor del periodo / población 2025 × 100. Se usa el mismo denominador para todos los periodos.
  - % fibra óptica = conexiones de fibra / conexiones totales. No depende del filtro de tecnología.
  - **IHH** = Σ (cuota de cada empresa en %)², de 0 a 10,000. Referencia: menos de 1,500 no concentrado, de 1,500 a 2,500 moderado, más de 2,500 muy concentrado.
  - Variación interanual: respecto del mismo trimestre o mes del año anterior.
- **Escalas de color:** sextiles de la distribución de zonas (tono más oscuro = valor más alto); las variaciones usan una escala divergente con cortes fijos (−20, −5, 0, 5 y 20 %).

## Limitaciones

- La penetración se mide por habitante, no por hogar. En internet fijo, una conexión suele atender a todo un hogar.
- **Líneas móviles:** las líneas «SIN LAC» (sin código de área de localización, hasta 2022) suman al total nacional, pero no se asignan a ningún departamento.
- **Reclamos:** OSIPTEL reporta Lima y Callao juntos, por lo que Callao se muestra como «incluido en Lima». Antes de 2023 los reclamos no tienen detalle departamental.
- El último mes de cada serie puede corresponder a cifras preliminares de OSIPTEL.

## Créditos

Elaboración: **Good Guys Insight**. Datos: OSIPTEL (PUNKU) e INEI (Censos Nacionales 2025). Límites: SDOT–PCM (RENLIM). Última actualización: **16 de octubre de 2026**.
