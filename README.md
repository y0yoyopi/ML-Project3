# Localización Indoor mediante Huellas WiFi y Clustering

## Descripción

En entornos interiores donde las señales GPS no están disponibles (como centros comerciales y edificios de oficinas), el posicionamiento por huella digital basado en la intensidad de la señal WiFi (RSSI) es una tecnología ampliamente utilizada.

Este proyecto, basado en el reconocido dataset **UJIIndoorLoc**, utiliza algoritmos de clustering de machine learning para analizar las características de las señales WiFi recibidas en diferentes ubicaciones e identificar las áreas potenciales donde se encuentra un usuario.

---

## Objetivos del Proyecto

- Procesar datos de señales WiFi de alta dimensionalidad (520 puntos de acceso por lectura)
- Aplicar técnicas de reducción de dimensionalidad (PCA, t-SNE) para visualización
- Implementar aprendizaje no supervisado (clustering)
- Evaluar el rendimiento con métricas internas y externas
- Explorar la formación automática de clusters espaciales significativos

---

### Requisitos Previos

- Docker
- Docker Compose
- Git

---

## Instalación

### Clonar el repositorio

```bash
git clone https://github.com/tuusuario/indoor-localization-clustering.git
cd indoor-localization-clustering
```

---

### Descargar el dataset

Descargar:

- `trainingData.csv`
- `validationData.csv`

Desde el repositorio oficial de UCI.

---

### Colocar los datos en la carpeta correspondiente

```bash
mkdir -p data
cp /ruta/a/tu/descarga/trainingData.csv data/
cp /ruta/a/tu/descarga/validationData.csv data/
```

---

### Construir y ejecutar el contenedor

```bash
docker-compose build
docker-compose up -d
```

---

### Acceder a Jupyter

Abrir en el navegador:

````
http://localhost:8888
````

---

### Detener el contenedor

```bash
docker-compose down
```

---

## Estructura del Proyecto

````
indoor-localization-clustering/
├── .gitignore
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
├── README.md
├── data/
│   ├── trainingData.csv
│   ├── validationData.csv
│   └── .gitkeep
└── notebooks/
    └── analisis.ipynb
````

---

## Metodología

### Análisis Exploratorio (EDA)

- Distribución de señales RSSI  
- Densidad de detección de WAPs  
- Proyección espacial real de muestras  
- PCA inicial para visualización  

---

### Preprocesamiento

- Reemplazo de valores 100 por -110  
- Eliminación de WAPs con varianza muy baja  
- Estandarización con StandardScaler  
- PCA conservando 95% de varianza  

---

### Clustering con K-Means

Determinación de K óptimo mediante:

- Método del codo  
- Silhouette score  

Evaluación para:

`K ∈ {2, 3, 6, 10, 18, 20}`

---

## Resultados Principales

| K  | Silhouette | ARI  | NMI  |
|----|------------|------|------|
| 2  | 0.532      | 0.002| 0.032|
| 3  | 0.093      | 0.165| 0.420|
| 6  | 0.145      | 0.245| 0.520|
| 10 | 0.120      | 0.429| 0.623|
| 18 | 0.172      | 0.301| 0.558|
| 20 | 0.170      | 0.278| 0.556|

---

## Hallazgos Clave

- **K=2** maximiza separación geométrica pero no corresponde a áreas reales  
- **K=10** ofrece mejor equilibrio (ARI=0.429, NMI=0.623)  
- Las métricas internas muestran superposición significativa  
- Las métricas externas revelan estructura latente parcialmente capturada por K-Means  
