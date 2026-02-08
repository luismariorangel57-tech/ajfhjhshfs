# Trabajo #1: Análisis de Face Tracking y Visión por Computadora

## 1. Introducción a las Tecnologías de Visión Web

### ¿Qué es MindAR?
MindAR es una biblioteca de software de código abierto y ligera diseñada para desarrollar experiencias de **Realidad Aumentada (AR)** en la web. Permite el reconocimiento de imágenes y seguimiento facial directamente en el navegador utilizando tecnologías estándar como **WebGL** y **WebAssembly**, eliminando la necesidad de instalar aplicaciones externas.

### ¿Qué es OpenCV?
**OpenCV** (Open Source Computer Vision Library) es la biblioteca de visión artificial más utilizada a nivel mundial. Provee una infraestructura común para aplicaciones de visión por computadora y contiene más de 2500 algoritmos optimizados para tareas como detección de rostros, identificación de objetos y procesamiento de imágenes.

### ¿MindAR usa OpenCV?
**No.** Aunque ambas procesan imágenes, MindAR no depende de OpenCV. Está construida sobre **TensorFlow.js** y utiliza modelos de aprendizaje profundo (Deep Learning). Mientras OpenCV se basa en algoritmos clásicos de procesamiento de matrices de píxeles, MindAR se basa en inferencia de redes neuronales.

---

## 2. Detección de Bordes y Modelos Matemáticos

### Algoritmo de Canny Edge Detection
Es una técnica de procesamiento de imágenes utilizada para detectar bordes de manera robusta. Se considera el estándar óptimo porque cumple con baja tasa de error, localización precisa y respuesta única por cada borde.

#### Uso de Diferencias Finitas
Dado que una imagen digital es una matriz discreta de píxeles, el algoritmo utiliza **Diferencias Finitas** para aproximar el gradiente (derivada) de la intensidad de la imagen:

$$f'(x) \approx f(x+1) - f(x-1)$$



### Operador Sobel
El algoritmo de Sobel calcula una aproximación del gradiente de intensidad. Utiliza máscaras (kernels) de $3 \times 3$ para estimar el gradiente en el eje horizontal ($G_x$) y vertical ($G_y$). La combinación de ambos permite obtener la magnitud total del borde:

$$G = \sqrt{G_x^2 + G_y^2}$$



---

## 3. Implementaciones Técnicas

### A. Detección de Bordes Sobel (OpenCV.js)
Esta implementación realiza el procesamiento de video en tiempo real convirtiendo cada frame a escala de grises y aplicando el operador Sobel.

> **Archivo:** `filtro_sobel.html`

```javascript
// Paso fundamental: Conversión a Escala de Grises
cv.cvtColor(src, src, cv.COLOR_RGBA2GRAY);

// Aplicación del Algoritmo Sobel (dx=1, dy=0 para bordes verticales)
cv.Sobel(src, dst, cv.CV_8U, 1, 0, 3, 1, 0, cv.BORDER_DEFAULT);

cv.imshow('canvasOutput', dst);
# 🎥 Demostración Interactiva

Para ver el código funcionando en tiempo real con tu webcam, haz clic en los siguientes botones:

[![Probar Face Tracking](https://img.shields.io/badge/EJECUTAR-FACE_TRACKING-blue?style=for-the-badge&logo=googlechrome&logoColor=white)](https://TU_USUARIO.github.io/NOMBRE_DE_TU_REPO/face_tracking.html)

[![Probar Filtro Sobel](https://img.shields.io/badge/EJECUTAR-FILTRO_SOBEL-green?style=for-the-badge&logo=opencv&logoColor=white)](https://TU_USUARIO.github.io/NOMBRE_DE_TU_REPO/filtro_sobel.html)

> **Nota:** Es necesario otorgar permisos de cámara al navegador para que las demostraciones funcionen.
---
