# Trabajo #1: Análisis de Face Tracking y Visión por Computadora

## 1. Conceptos Fundamentales

### ¿Qué es MindAR?
Es una biblioteca de software de código abierto y ligera diseñada para desarrollar experiencias de **Realidad Aumentada (AR)** en la web. Permite el seguimiento facial y de imágenes directamente en el navegador usando WebGL y WebAssembly.

### ¿Qué es OpenCV?
**OpenCV** (Open Source Computer Vision Library) es la biblioteca de visión artificial más usada a nivel mundial. Contiene más de 2500 algoritmos optimizados para detección de rostros, objetos y procesamiento de imágenes.

### ¿MindAR usa OpenCV?
**No.** MindAR está construida sobre **TensorFlow.js** y usa modelos de Deep Learning propios. OpenCV se basa en algoritmos clásicos de procesamiento de píxeles, mientras que MindAR usa inferencia de redes neuronales.

---

## 2. Análisis del Algoritmo Canny Edge Detection

El algoritmo de Canny es el estándar óptimo para detectar bordes debido a su precisión y bajo error.

### Uso de Matemáticas y Diferencias Finitas
En una imagen (que es una matriz discreta), no podemos calcular derivadas analíticas. Por ello, el algoritmo usa **Diferencias Finitas** para aproximar el gradiente de intensidad:

$$f'(x) \approx f(x+1) - f(x-1)$$

[Image of Canny edge detection steps: Noise reduction, Gradient calculation, Non-maximum suppression, Hysteresis thresholding]

---

## 3. Algoritmo de Sobel

El operador Sobel calcula una aproximación del gradiente de intensidad. Utiliza máscaras de $3 \times 3$ para obtener los gradientes vertical ($G_y$) y horizontal ($G_x$).

### Magnitud del Gradiente
La magnitud total del borde se calcula mediante la fórmula de la hipotenusa:

$$G = \sqrt{G_x^2 + G_y^2}$$

[Image of Sobel operator kernels for horizontal and vertical edge detection]

---

## 4. Implementaciones Técnicas (Código Fuente)

### A. Detección de Bordes con OpenCV.js (Punto 23)
Este código utiliza la webcam para aplicar el filtro Sobel en tiempo real.

```html
<!DOCTYPE html>
<html>
<head>
    <title>Detección de Bordes Sobel en Vivo</title>
    <script async src="[https://docs.opencv.org/4.8.0/opencv.js](https://docs.opencv.org/4.8.0/opencv.js)" onload="main()"></script>
    <style>body { background: #111; color: white; text-align: center; }</style>
</head>
<body>
    <h3>Filtro Sobel en Tiempo Real (OpenCV.js)</h3>
    <video id="videoInput" width="320" height="240" style="display:none"></video>
    <canvas id="canvasOutput" width="320" height="240"></canvas>

    <script>
        function main() {
            const video = document.getElementById("videoInput");
            navigator.mediaDevices.getUserMedia({ video: true, audio: false })
            .then(function(stream) {
                video.srcObject = stream;
                video.play();
                video.onloadedmetadata = () => { setTimeout(processVideo, 100); };
            });

            function processVideo() {
                const src = new cv.Mat(video.height, video.width, cv.CV_8UC4);
                const dst = new cv.Mat(video.height, video.width, cv.CV_8UC1);
                const cap = new cv.VideoCapture(video);
                function loop() {
                    try {
                        if (video.paused || video.ended) return;
                        cap.read(src);
                        cv.cvtColor(src, src, cv.COLOR_RGBA2GRAY);
                        cv.Sobel(src, dst, cv.CV_8U, 1, 0, 3, 1, 0, cv.BORDER_DEFAULT);
                        cv.imshow("canvasOutput", dst);
                        requestAnimationFrame(loop);
                    } catch (err) { console.error(err); }
                }
                loop();
            }
        }
    </script>
</body>
</html>
