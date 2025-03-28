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





