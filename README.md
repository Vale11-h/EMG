# SEÑALES ELECTROMIOGRÁFICAS EMG
La electromiografía (EMG) es una técnica diagnóstica y de investigación que permite evaluar la actividad eléctrica generada por las células musculares durante su contracción, proporcionando información fundamental sobre la función neuromuscular. Mediante electrodos colocados en la piel o insertados directamente en el músculo, este método registra los potenciales de acción de las unidades motoras, lo que permite a los investigadores y profesionales de la salud detectar alteraciones neuromusculares, diagnosticar enfermedades como distrofias musculares, neuropatías periféricas y lesiones de nervios, así como estudiar la respuesta muscular en diferentes condiciones experimentales. En un contexto de laboratorio, la EMG se utiliza tanto para propósitos clínicos como para investigación básica, facilitando la comprensión de los mecanismos de contracción muscular, la coordinación neuromuscular y la evaluación cuantitativa de la función muscular en condiciones normales y patológicas.

### CAPTURA DE LA SEÑAL

En primer lugar, para la adquisición de la señal electromiográfica (EMG), se debe preparar adecuadamente al sujeto, asegurando una correcta colocación de los electrodos de superficie sobre el músculo seleccionado, utilizando gel conductor para optimizar la conductividad y minimizar el ruido en la señal. Posteriormente, los electrodos deben conectarse al sistema de adquisición de datos (DAQ), como el NI USB-6001/6002/6003, previa instalación del controlador NI-DAQmx y su API para Python, además de la revisión de la documentación correspondiente.

![WhatsApp Image 2025-03-27 at 22 11 36](https://github.com/user-attachments/assets/db48da2e-b782-470d-9ed2-4f8ad7ee130a)
> Interfaz gráfica realizada en Python, donde se capturó y guardó la señal.

La frecuencia de muestreo debe calcularse con base en el músculo estudiado para garantizar una captura precisa de la señal. Durante el experimento, se seleccionaron dos músculos: el extensor radial del carpo y el flexor radial del carpo, que serán objeto de estudio. Se solicita al sujeto que realice una contracción muscular sostenida hasta la fatiga, mientras el sistema registra en tiempo real la actividad EMG, permitiendo el análisis de la respuesta neuromuscular y la evolución de la fatiga mediante el procesamiento de la señal adquirida.

<img src="https://github.com/user-attachments/assets/22a89ccf-6d10-4e8d-8dbb-4932db88437e" width="300">.
> Ubicación de electrodos.

Captura de EMG con frecuencia de muestreo entre 1000 Hz y 2000 Hz, este valor es importante ya que de este depende la calidad y precisión de los datos recolectados, de manera que esta frecuencia es recomendable para poder capturar correctamente las señales eléctricas generadas por los músculos.

![q](https://github.com/user-attachments/assets/439929bd-7543-47fe-9369-544c3b824616)
> Datos de la señal EMG graficados.
> > Eje Horizontal (Tiempo [s]): Esta dimensión horizontal ilustra la evolución temporal de la actividad bioeléctrica muscular, permitiendo visualizar con precisión cronológica los momentos de activación, intensidad y relajación de los grupos musculares involucrados en el movimiento.

> > Eje de Ordenadas (Amplitud [mV]): este eje cuantifica la magnitud de la señal eléctrica capturada, reflejando la intensidad y el volumen de los potenciales de acción producidos por las diferentes unidades motoras presentes en el tejido muscular.

### FILTRADO DE LA SEÑAL
El procesamiento de la señal electromiográfica requirió una estrategia de filtrado sofisticada que combinó un filtro pasa-banda mediante la integración de componentes pasa-alto y pasa-bajo. La metodología implementada se centró en depurar la señal, eliminando frecuencias no deseadas tanto en el espectro superior como inferior.

El diseño contempló un filtro pasa-bajo con una frecuencia de corte de 10 Hz y una atenuación específica de 60 Hz. Posteriormente, se realizó la normalización de la expresión, convirtiendo hercios a radianes por segundo y definiendo la amplitud en decibelios. El análisis determinó un filtro de cuarto orden, fundamentado en el polinomio característico del filtro Butterworth.

La configuración final estableció un filtro pasa-banda con rangos de frecuencia entre 10 Hz y 500 Hz, lo que permitió capturar selectivamente las señales electromiográficas más relevantes, optimizando la calidad del registro y minimizando las interferencias espectrales.

> **Diseño del filtro:**

> > **•	Frecuencia de corte baja (fL):** 10 Hz

> > **•	Frecuencia de corte alta (fH):** 500 Hz

> > **•	Frecuencia de muestreo (fs):** 1000 Hz

> > **•	Atenuación fuera de la banda:** 60 Hz

> > **•	Orden del filtro:** Butterworth orden 4

![h](https://github.com/user-attachments/assets/89791bc4-fe1f-47c8-8734-ea3ba8b96d4d)
> Señal filtrada.

### AVENTAMIENTO

Durante el análisis de la actividad muscular, se implementó la ventana de Hamming, una técnica matemática sofisticada que permite descomponer la señal bioeléctrica con precisión milimétrica. Esta herramienta especializada actúa como un filtro inteligente que suaviza los bordes de la señal, eliminando discontinuidades y reduciendo las interferencias espectrales que pueden distorsionar la información original. La metodología consiste en aplicar la función de Hamming sobre el registro electromiográfico, dividiendo el conjunto de datos en segmentos más pequeños y uniformes. Al multiplicar la señal original por esta función de peso, se logra atenuar las oscilaciones abruptas en los límites de cada intervalo, lo que permite una representación más limpia y precisa de la actividad muscular durante el esfuerzo físico.

![k](https://github.com/user-attachments/assets/ccfa3c48-587d-4a8a-b8e2-db27e6a89619)
> Señal con ventanas.

Posteriormente, se utilizó la Transformada de Fourier para convertir la señal del dominio temporal al dominio de frecuencia. Este algoritmo matemático descompone la señal en sus componentes frecuenciales fundamentales, revelando patrones ocultos de activación neuromuscular que no son perceptibles en el análisis temporal directo. La transformación permite identificar con precisión las frecuencias dominantes y las características espectrales específicas de la contracción muscular. El protocolo estableció una frecuencia de corte de 1000 Hz, lo que garantiza la captura de las señales más relevantes sin perder información crítica. Se procesaron segmentos de 250 milisegundos, completando un total de 24 ventanas que corresponden a las repeticiones del ejercicio hasta alcanzar el punto de fatiga muscular.
Esta aproximación metodológica permite una descomposición detallada de la señal, aislando y resaltando los momentos más representativos del esfuerzo, y proporcionando una comprensión más profunda de la dinámica de activación muscular.

![y](https://github.com/user-attachments/assets/c1cd35c2-371a-4985-ae66-380f6ed1285b)
> Convolución por ventana.

### Análisis Espectral

Cuando el organismo enfrenta una demanda de esfuerzo sostenido, se desencadena una cascada de respuestas biológicas. Inicialmente, el sistema neuromuscular despliega una estrategia de máxima eficiencia, movilizando recursos y activando diversos grupos celulares para sostener la tensión requerida.No obstante, esta capacidad de respuesta no es infinita. Gradualmente, se produce una degradación del rendimiento muscular. La respuesta eléctrica experimenta una transformación: desde una señal robusta y expansiva, evoluciona hacia una manifestación más tenue y fragmentada.

Este declive no es simplemente una reducción numérica, sino un complejo proceso de agotamiento celular. Las fibras musculares, inicialmente coordinadas y sincronizadas, comienzan a desorganizarse, generando una señal cada vez más errática e inestable. La representación gráfica de este fenómeno revela una narrativa de resistencia y límite, donde el cuerpo transita desde un estado de máxima potencia hacia una condición de agotamiento progresivo. Cada variación en la intensidad eléctrica cuenta una historia de adaptación, resistencia y eventual rendición fisiológica.

![o](https://github.com/user-attachments/assets/dc38e07f-2fac-4c8e-b69c-0e8327aacd3c)
> Frecuencia mediana en función del tiempo.

 > **Ejes de la Gráfica:**

> > **-El eje horizontal (Ventanas):** Números de ventanas.
> > 
> > **-El eje vertical (Frecuencia [Hz]):** Frecuencias medianas medidas con respecto a las ventanas.

• La frecuencia mediana es un indicador electromiográfico que divide el espectro de potencia de la señal en dos áreas iguales, revelando cambios en la actividad muscular y siendo un parámetro fundamental para evaluar la fatiga y el comportamiento neuromuscular.

• La frecuencia dominante representa el rango de oscilaciones donde se concentra la mayor parte de la energía eléctrica muscular. Es como el "tono principal" de la actividad muscular, el punto donde la señal emite su máxima intensidad y muestra con mayor claridad el comportamiento eléctrico del músculo durante la contracción.

• La frecuencia media es un parámetro estadístico que calcula el promedio de todas las oscilaciones eléctricas presentes en una señal muscular durante un periodo determinado. Funciona como una especie de "centro de gravedad" de las frecuencias, proporcionando una visión general de cómo se distribuyen las vibraciones eléctricas a lo largo del tiempo.

• La desviación estándar es una medida matemática que indica cuánto se alejan las diferentes frecuencias de su valor promedio.
Una desviación estándar baja sugiere que las frecuencias están muy agrupadas y son uniformes. Una desviación estándar alta indica una gran variabilidad, con frecuencias muy dispersas y menos predecibles.

En esencia, estos parámetros permiten "fotografiar" la actividad eléctrica muscular desde diferentes ángulos, revelando su complejidad y comportamiento dinámico.

> **Datos estadisticos obtenidos por cada ventana:**
> > **-Ventana 1:** Frecuencia Mediana: 148.00 Hz, Frecuencia Dominante: 124.00 Hz, Frecuencia Media: 164.99 Hz, Desviación Estándar: 144.91 Hz
> > 
> > **-Ventana 2:** Frecuencia Mediana: 220.00 Hz, Frecuencia Dominante: 220.00 Hz, Frecuencia Media: 222.11 Hz, Desviación Estándar: 144.91 Hz
> > 
> > **-Ventana 3:** Frecuencia Mediana: 164.00 Hz, Frecuencia Dominante: 168.00 Hz, Frecuencia Media: 164.57 Hz, Desviación Estándar: 144.91 Hz
> > 
> > **-Ventana 4:** Frecuencia Mediana: 152.00 Hz, Frecuencia Dominante: 152.00 Hz, Frecuencia Media: 174.34 Hz, Desviación Estándar: 144.91 Hz
> > 
> > **-Ventana 5:** Frecuencia Mediana: 148.00 Hz, Frecuencia Dominante: 112.00 Hz, Frecuencia Media: 182.24 Hz, Desviación Estándar: 144.91 Hz
> > 
> > **-Ventana 6:** Frecuencia Mediana: 184.00 Hz, Frecuencia Dominante: 104.00 Hz, Frecuencia Media: 191.83 Hz, Desviación Estándar: 144.91 Hz
> > 
> > **-Ventana 7:** Frecuencia Mediana: 144.00 Hz, Frecuencia Dominante: 140.00 Hz, Frecuencia Media: 177.40 Hz, Desviación Estándar: 144.91 Hz
> > 
> > **-Ventana 8:** Frecuencia Mediana: 184.00 Hz, Frecuencia Dominante: 148.00 Hz, Frecuencia Media: 194.90 Hz, Desviación Estándar: 144.91 Hz
> > 
> > **-Ventana 9:** Frecuencia Mediana: 160.00 Hz, Frecuencia Dominante: 156.00 Hz, Frecuencia Media: 195.25 Hz, Desviación Estándar: 144.91 Hz
> > 
> > **-Ventana 10:** Frecuencia Mediana: 192.00 Hz, Frecuencia Dominante: 188.00 Hz, Frecuencia Media: 202.92 Hz, Desviación Estándar: 144.91 Hz
> > 
> > **-Ventana 11:** Frecuencia Mediana: 240.00 Hz, Frecuencia Dominante: 244.00 Hz, Frecuencia Media: 216.03 Hz, Desviación Estándar: 144.91 Hz
> > 
> > **-Ventana 12:** Frecuencia Mediana: 220.00 Hz, Frecuencia Dominante: 208.00 Hz, Frecuencia Media: 230.65 Hz, Desviación Estándar: 144.91 Hz
> > 
> > **-Ventana 13:** Frecuencia Mediana: 220.00 Hz, Frecuencia Dominante: 252.00 Hz, Frecuencia Media: 217.40 Hz, Desviación Estándar: 144.91 Hz
> > 
> > **-Ventana 14:** Frecuencia Mediana: 200.00 Hz, Frecuencia Dominante: 128.00 Hz, Frecuencia Media: 215.17 Hz, Desviación Estándar: 144.91 Hz
> > 
> > **-Ventana 15:** Frecuencia Mediana: 208.00 Hz, Frecuencia Dominante: 204.00 Hz, Frecuencia Media: 208.01 Hz, Desviación Estándar: 144.91 Hz
> > 
> > **-Ventana 16:** Frecuencia Mediana: 200.00 Hz, Frecuencia Dominante: 304.00 Hz, Frecuencia Media: 210.53 Hz, Desviación Estándar: 144.91 Hz
> > 
> > **-Ventana 17:** Frecuencia Mediana: 212.00 Hz, Frecuencia Dominante: 184.00 Hz, Frecuencia Media: 221.21 Hz, Desviación Estándar: 144.91 Hz
> > 
> > **-Ventana 18:** Frecuencia Mediana: 216.00 Hz, Frecuencia Dominante: 216.00 Hz, Frecuencia Media: 213.18 Hz, Desviación Estándar: 144.91 Hz
> > 
> > **-Ventana 19:** Frecuencia Mediana: 232.00 Hz, Frecuencia Dominante: 248.00 Hz, Frecuencia Media: 217.16 Hz, Desviación Estándar: 144.91 Hz
> > 
> > **-Ventana 20:** Frecuencia Mediana: 216.00 Hz, Frecuencia Dominante: 220.00 Hz, Frecuencia Media: 217.40 Hz, Desviación Estándar: 144.91 Hz
> > 
> > **-Ventana 21:** Frecuencia Mediana: 164.00 Hz, Frecuencia Dominante: 132.00 Hz, Frecuencia Media: 177.50 Hz, Desviación Estándar: 144.91 Hz
> > 
> > **-Ventana 22:** Frecuencia Mediana: 192.00 Hz, Frecuencia Dominante: 108.00 Hz, Frecuencia Media: 189.78 Hz, Desviación Estándar: 144.91 Hz
> > 
> > **-Ventana 23:** Frecuencia Mediana: 152.00 Hz, Frecuencia Dominante: 136.00 Hz, Frecuencia Media: 173.11 Hz, Desviación Estándar: 144.91 Hz
> > 
> > **-Ventana 24:** Frecuencia Mediana: 120.00 Hz, Frecuencia Dominante: 116.00 Hz, Frecuencia Media: 159.53 Hz, Desviación Estándar: 144.91 Hz

![P](https://github.com/user-attachments/assets/07fa18db-3297-45dc-b15b-6ad635241ce0)
> Análisis espectral, por ventanas.
> **Teniendo en cuenta todas las ventanas:**

> > **-Frecuencia dominante:** 154.00 Hz

> > **-Desviación estándar de las frecuencias medianas:** 32.19 Hz

> > **-Frecuencia media:** 187.00 Hz

![r](https://github.com/user-attachments/assets/7afd5dda-c1fb-42c1-a5f7-7c81c5f8df40)
> Espectro filtrado de la señal.
> **Ejes de la Gráfica:**
> > **-El eje horizontal (Frecuencia [Hz]):** Diferentes frecuencias presentes en la señal.

> > **-El eje vertical (Potencia [dB]):** cantidad de potencia asociada con cada frecuencia.

### PRUEBA DE HIPÓTESIS 

La prueba de hipótesis en el contexto del análisis electromiográfico constituye una herramienta estadística fundamental que permite validar científicamente los cambios observados en la señal muscular. Este método riguroso implica formular una hipótesis nula (que supone ausencia de cambios significativos) y una hipótesis alternativa (que propone la existencia de transformaciones relevantes)

 ###### Valor p de la prueba de hipótesis: 0.4111
 
Al aplicar la prueba de hipótesis en el análisis del código como último paso del procedimiento, se obtuvo un valor p de 0.4111. Éste sugiere que los cambios que se vieron en las señales de EMG a lo largo de cada contracción hasta llegar a la fatiga muscular no son significativos estadísticamente. Dado que el valor p de 0.4111 es mayor que el nivel de significancia que suele ser usado (0.05), no se rechaza la hipótesis nula. De este modo, no hay evidencia estadística suficiente para asegurar que la fatiga muscular tiene un importante efecto sobre la frecuencia mediana de las señales EMG. En consecuencia, se puede concluir que el protocolo que se implementó para la toma de la señal EMG y los movimientos realizados no produjeron un efecto notable sobre la actividad eléctrica del músculo.

### Instrucciones para la Ejecución del Laboratorio}

**1. Entorno de ejecución:**

El laboratorio se realizó en Google Colab, un entorno basado en la nube que permite ejecutar código Python sin necesidad de configuraciones locales avanzadas.

#### Librerías utilizadas:

Se instalaron y usaron las siguientes librerías en Python:

###### • PyQt5: Para la creación de interfaces gráficas.
###### • pyqtgraph: Para la visualización de señales en gráficos interactivos.
###### • numpy: Para operaciones matemáticas y manipulación de arreglos.
###### • scipy: Para procesamiento de señales (filtros, transformadas, etc.).
###### • matplotlib: Para generación de gráficos y visualización de datos.

**2. Instalación de librerías en Colab:**

Dado que Google Colab no incluye por defecto PyQt5, se instaló manualmente junto con las demás dependencias necesarias.
Se utilizó un entorno virtual gráfico para permitir la ejecución de interfaces gráficas en la nube.

**3. Estructura del código:**

• Se definió una interfaz gráfica en PyQt5 con un QMainWindow para cargar y visualizar señales.
• Se implementó la funcionalidad de carga de archivos en formato txt.
• Se añadió una sección para aplicar filtros a la señal cargada y visualizar los resultados.
• Una vez instaladas las dependencias y configurado el entorno gráfico, se ejecuta el código dentro de una celda de Colab.

## Requisitos

• Tener Python 3.9 instalado y utilizar Google Colab (o cualquier compilador compatible).

• Tener acceso a los archivos .dat y .hea para cargar en Google Colab o el compilador elegido.

• Contar con las librerías necesarias instaladas para ejecutar el código correctamente (especificadas en el inicio del artículo).

## Usar

Por favor, cite este artículo:

Huertas, V.; Ramírez, P.; Delgado, A. Análisis estadístico de la señal. 6 de febrero de 2025.

## Información de contacto

• est.laurav.huertas@unimilitar.edu.co
• est.deisy.aramirez@unimilitar.edu.co
• est.paulav.gomez@unimilitar.edu.co
