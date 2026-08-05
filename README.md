# Laboratorio 1: Monitoreo del patrón y frecuencia respiratoria
### Integrantes:
Lina María Cortes Almonacid  
Karen Dayanna Mora Segura  
Sofia Alejandra Cardona Cruz     

En esta práctica se desarrolló un sistema para monitorear el patrón respiratorio de una persona en reposo y durante el habla, con el propósito de observar cómo la verbalización modifica la forma y la frecuencia de la respiración. Para la adquisición se utilizó un micrófono KY instalado dentro de un tapabocas y conectado a una ESP32, la cual registró las variaciones de sonido producidas durante la inhalación, la exhalación y el habla. La señal fue procesada mediante filtros y cálculos de energía para reducir el ruido y diferenciar aproximadamente entre silencio, respiración y voz, siguiendo el objetivo planteado en la guía de comparar ambas condiciones respiratorias [1].  

Posteriormente, los datos fueron enviados por comunicación serial a MATLAB, donde se realizó una captura temporizada de 30 segundos, se almacenaron en archivos .mat y se representaron gráficamente. Aunque las señales obtenidas no presentaron completamente las formas esperadas, sí permitieron identificar cambios de amplitud y comportamiento entre la respiración en reposo y durante el habla. Las diferencias observadas pudieron estar relacionadas con el ruido del ambiente, la sensibilidad y ubicación del micrófono, el ajuste de los filtros y las variaciones naturales de la respiración del sujeto [2].  
### Objetivo General  
Evaluar la influencia del habla sobre el patrón y la frecuencia respiratoria mediante un sistema de adquisición basado en un micrófono KY, una ESP32 y MATLAB, comparando las señales obtenidas en condiciones de reposo y durante la verbalización.  

### Objetivos específicos  
- Reconocer las variaciones físicas y acústicas producidas durante la inhalación, la exhalación y el habla.  
- Diseñar e implementar un sistema capaz de adquirir la señal respiratoria mediante un micrófono KY conectado a una ESP32.  
- Procesar y filtrar la señal adquirida para reducir el ruido y facilitar la identificación del patrón respiratorio.  
- Capturar, almacenar y representar gráficamente en MATLAB las señales obtenidas durante 30 segundos en reposo y durante el habla.  
- Comparar el comportamiento y la frecuencia de las señales respiratorias obtenidas en ambas condiciones, identificando las diferencias producidas por la verbalización.  
### Metodología  
La práctica se desarrolló mediante la construcción de un sistema básico para registrar los sonidos producidos por la respiración y observar sus cambios cuando la persona se encontraba en reposo y cuando hablaba. Para ello, se utilizó un micrófono KY como elemento sensor, una tarjeta ESP32 para adquirir y procesar la señal, y MATLAB para guardar y representar los datos. El procedimiento se dividió en las etapas de montaje del sistema, ubicación del sensor, adquisición de la señal, filtrado, transmisión serial, captura en MATLAB y comparación de los resultados. De acuerdo con la guía, se debían obtener registros separados en reposo y durante una tarea de verbalización, con una duración aproximada de 30 segundos para cada condición [1].  

Se seleccionó un módulo de micrófono KY debido a que permite detectar cambios en el nivel del sonido del ambiente. En esta aplicación, el micrófono no midió directamente el volumen de aire que entraba o salía de los pulmones, sino las variaciones acústicas producidas durante la inhalación, la exhalación y el habla. Cuando una persona respira cerca del micrófono, el movimiento del aire y el sonido respiratorio producen pequeñas variaciones en la salida analógica del sensor[2] . Durante el habla, estas variaciones suelen presentar una amplitud mayor y cambios más rápidos debido a la voz [3].  


Después el micrófono se conectó a la ESP32 utilizando su salida analógica. Las conexiones realizadas fueron las siguientes:  

- El pin de alimentación del micrófono se conectó a la salida de 3,3 V de la ESP32.  
- El pin de tierra del micrófono se conectó a uno de los pines GND de la ESP32.  
- La salida analógica del micrófono se conectó al pin GPIO 15 de la ESP32.  
- La ESP32 se conectó al computador mediante un cable USB.  

Antes de iniciar las pruebas, se verificó que todas las conexiones estuvieran firmes y que el micrófono estuviera alimentado correctamente. También se confirmó que la ESP32 fuera reconocida por el computador y que pudiera enviar información mediante el puerto serial, aunque existen sensores que permiten medir directamente el flujo de aire, el movimiento del tórax o la presión respiratoria, el micrófono KY fue empleado como una alternativa sencilla y de bajo costo para reconocer cambios relacionados con el patrón respiratorio [2].  

Por otro lado, el micrófono fue colocado en un tapabocas utilizado por la persona participante. Se ubicó cerca de la nariz y la boca con el propósito de captar los sonidos producidos durante la inhalación, la exhalación y la verbalización, durante la colocación se trato que el módulo no bloqueara la entrada o salida del aire y que no estuviera en contacto directo con la boca, la posición del sensor se conservó durante las pruebas de reposo y habla. Esto permitió que las diferencias observadas se relacionaran principalmente con la condición evaluada y no con un cambio en la distancia entre el micrófono y la persona. La prueba se realizó en un ambiente lo más silencioso posible. Sin embargo, debido a la alta sensibilidad del micrófono, algunos sonidos del entorno, el movimiento del tapabocas y la fricción del sensor pudieron quedar registrados como interferencias [2].  

La ESP32 se programó desde el entorno de Arduino IDE, en el código se configuró el pin GPIO 15 como entrada para leer la señal analógica entregada por el micrófono y la comunicación serial se configuró a una velocidad de 115200 baudios, utilizada tanto para observar las señales en el Serial Plotter como para enviar posteriormente los datos hacia MATLAB [4].  

En la parte de la programación al iniar la adquisición de datos, se realizaba una calibración, durante este intervalo, la persona debía permanecer en silencio y evitar movimientos fuertes cerca del micrófono. La ESP32 tomó 1000 lecturas del sensor y calculó el promedio, este promedio fue utilizado como nivel de referencia, esta calibración fue necesaria porque la salida del micrófono no se encuentra normalmente en cero cuando no existe sonido. En su lugar, presenta un valor central sobre el cual aparecen pequeñas variaciones. Al calcular este nivel inicial, fue posible restarlo de las lecturas posteriores y trabajar principalmente con los cambios generados por la respiración y la voz [5].  

La señal analógica del micrófono fue leída por la ESP32 cada 500 microsegundos. Esto corresponde a una frecuencia interna aproximada de:  
<img width="420" height="85" alt="image" src="https://github.com/user-attachments/assets/38841e6e-b2fd-45e6-a2f1-cd3eaf1e5838" />  
Por lo tanto, la ESP32 realizó aproximadamente 2000 lecturas por segundo. Esta velocidad permitió registrar las variaciones rápidas producidas por los sonidos respiratorios y la voz.

Para obtener la señal respiratoria, primero se eliminó el nivel constante o componente DC presente en la salida del micrófono, con el propósito de conservar principalmente las variaciones producidas por la respiración y la voz. Debido a que las lecturas individuales cambiaban rápidamente y presentaban una forma irregular, no se utilizaron directamente para representar el patrón respiratorio [5].  

Posteriormente, se calculó la energía RMS en grupos de 40 muestras, lo que permitió obtener una medida más estable de la intensidad del sonido y reducir las variaciones rápidas de la señal original [6]. Como la ESP32 adquiría 2000 muestras por segundo, cada ventana de 40 muestras correspondía aproximadamente a 20 milisegundos. Por esta razón, el valor RMS se actualizaba cerca de 50 veces por segundo, facilitando la observación y el procesamiento de los cambios asociados con la respiración.  

A partir de este procesamiento se generaron dos señales: una destinada a representar la respiración y otra a identificar los cambios producidos por el habla, a la señal respiratoria se le aplicó un filtro pasa-bajas IIR de segundo orden, este filtro permitió obtener una señal más lenta y continua, de modo que las inhalaciones y exhalaciones fueran más fáciles de identificar, mientras que la señal del habla se configuró para responder rápidamente cuando la persona comenzaba a hablar y disminuir de manera gradual al terminar. Además, se realizó una clasificación aproximada entre silencio, respiración y habla, según la intensidad detectada por el micrófono [5].  

Una vez comprobado el funcionamiento del sistema en el Serial Plotter de Arduino, se realizaron dos pruebas con el micrófono ubicado dentro del tapabocas y cerca de la nariz y la boca. En la primera prueba, la persona permaneció en reposo, respirando normalmente y evitando hablar o realizar movimientos que pudieran producir ruido. En la segunda prueba, la persona habló o leyó un texto mientras se registraban las variaciones de la señal. Durante ambas mediciones se mantuvo la misma posición del micrófono para que la comparación fuera más confiable. Finalmente, se observaron y compararon las señales obtenidas, esperando encontrar un comportamiento más suave y regular durante el reposo y una forma más irregular, con cambios rápidos y mayores variaciones durante el habla, tal como propone la guía para evaluar la influencia de la verbalización sobre el patrón respiratorio [1][3].  

Finalmente, en MATLAB se configuró la comunicación serial con la ESP32 mediante el puerto COM6 y una velocidad de 115200 baudios [7]. Luego se realizó la captura de la señal durante 30 segundos, almacenando cada dato recibido en un vector junto con su correspondiente tiempo. Finalmente, la información se guardó en un archivo .mat, se graficó la señal respiratoria en función del tiempo y se cerró la conexión serial.  



### Resultados  
En esta sección se presentan las señales obtenidas mediante el sistema de adquisición desarrollado, tanto durante la respiración en reposo como durante el habla. Los resultados incluyen las gráficas observadas en MATLAB durante 30 segundos. A partir de estas señales se analizaron los cambios en la amplitud, la forma y la regularidad del patrón respiratorio, así como las posibles diferencias producidas por la verbalización.  

En la primera gráfica se presentan las señales respiratorias obtenidas en condiciones de reposo y durante el habla. En reposo, la señal mostró variaciones relativamente suaves y varios ciclos reconocibles a lo largo del registro. Aunque el comportamiento no fue completamente periódico, se observaron aumentos y descensos que pueden relacionarse con las fases de inhalación y exhalación.  

<img width="713" height="447" alt="image" src="https://github.com/user-attachments/assets/d6faf912-af18-4221-906e-5eaf229257cc" />


Las Figuras 1 y 2 muestran las señales registradas durante la respiración en reposo y la respiración durante el habla, respectivamente. Aunque fue posible adquirir la señal mediante el micrófono KY-038 y diferenciar ambas condiciones, la calidad de los registros se vio afectada por la presencia de ruido y una falla 

Durante la adquisición de datos fue necesario utilizar tapabocas, lo que produjo turbulencias en el flujo de aire cerca del micrófono y modificó las características acústicas de la respiración. Adicionalmente, el ambiente presentaba diferentes fuentes de ruido que fueron captadas por el sensor, generando fluctuaciones que dificultaron la identificación clara de cada ciclo respiratorio.

A pesar de estas limitaciones, se observa que la respiración durante el habla la señal presenta una mayor amplitud y variabilidad en comparación con la respiración en reposo, comportamiento esperado debido a la producción de la voz y a la modificación del flujo respiratorio durante el habla.

<img width="690" height="441" alt="image" src="https://github.com/user-attachments/assets/1fa9773d-adda-409a-a649-46db494831d3" />

En esta imagen podemos observar la figura 3 y 4 donde en reposo se identifica una componente dominante alrededor de 0.20 Hz, mientras que la respiración durante el habla aparecen varios picos distribuidos en el espectro, indicando que la energía de la señal ya no está concentrada únicamente en la frecuencia respiratoria sino también en las frecuencias gebneradas por la voz.


### Discusión   
1. ¿Son los patrones respiratorios y las frecuencias respiratorias iguales o diferentes en cada caso? ¿A qué se debe esto?

No, los patrones respiratorios y la frecuencia respiratoria no fueron iguales en las dos condiciones evaluadas (reposo y verbalización). Durante el reposo se observó una respiración más constante y periódica, con ciclos de inhalación y exhalación relativamente uniformes. En cambio, durante la verbalización el patrón respiratorio presentó cambios debido a que el sujeto debía coordinar la respiración con el habla, generando exhalaciones más prolongadas e inhalaciones más cortas y menos frecuentes [1].  


Estas diferencias se deben a que la respiración no solo cumple la función de realizar el intercambio de gases, sino que también participa en la producción de la voz. Cuando una persona habla, el sistema respiratorio adapta el ritmo respiratorio para suministrar el flujo de aire necesario durante la fonación, modificando temporalmente la frecuencia y el patrón normal de respiración [2]. Por esta razón, es esperado que las señales respiratorias obtenidas durante la verbalización presenten un comportamiento diferente al registrado en reposo.  

2. ¿Cuáles serían las ventajas y desventajas de emplear múltiples sensores para el monitoreo del proceso respiratorio? ¿Cuáles podrían ser las razones?

El uso de múltiples sensores puede mejorar la calidad del monitoreo respiratorio, ya que permite obtener información desde diferentes variables fisiológicas. Por ejemplo, un sensor de CO₂ puede complementarse con sensores de flujo de aire, temperatura o movimiento torácico para obtener una descripción más completa del proceso respiratorio y aumentar la confiabilidad de las mediciones.  

Sin embargo, utilizar varios sensores también presenta algunas desventajas. En primer lugar, aumenta el costo del sistema y la complejidad del montaje. Además, es necesario sincronizar correctamente las señales obtenidas por cada sensor para evitar errores durante el análisis. También puede resultar menos cómodo para el usuario si se requiere colocar varios dispositivos sobre el cuerpo, lo que podría afectar la naturalidad de la respiración y, por lo tanto, influir en los resultados obtenidos.  

En general, la elección entre utilizar uno o varios sensores dependerá de la aplicación. Para un monitoreo básico de la frecuencia respiratoria, un solo sensor puede ser suficiente; mientras que para aplicaciones clínicas o de investigación, el uso de múltiples sensores permite obtener información más completa y reducir la posibilidad de errores en la interpretación de la señal.  

### Conclusiones  

### Referencias  
[1] Universidad Militar Nueva Granada. (2025). Guía de práctica de laboratorio: Monitoreo del patrón y frecuencia respiratoria.   
[2] Romano, C., Nicolò, A., Innocenti, L., Bravi, M., Miccinilli, S., Sterzi, S., Sacchetti, M., Schena, E., & Massaroni, C. (2023). Respiratory rate estimation during walking and running using breathing sounds recorded with a microphone. Biosensors, 13(6), 637. https://doi.org/10.3390/bios13060637  

[3] Fuchs, S., & Rochet-Capellan, A. (2021). The respiratory foundations of spoken language. Annual Review of Linguistics, 7, 13–30. https://doi.org/10.1146/annurev-linguistics-031720-103907  

[4] Espressif Systems. (s. f.). Analog to digital converter (ADC)—ESP32 programming guide. https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/peripherals/adc/index.html  

[5] Microchip Technology Inc. (2022). Processing analog sensor data with digital filtering (Application Note AN4515). https://www.microchip.com/en-us/application-notes/an4515  

[6] MathWorks. (s. f.). rms: Root mean square value. https://www.mathworks.com/help/matlab/ref/rms.html  

[7] MathWorks. (s. f.). serialport: Connection to serial port. https://www.mathworks.com/help/matlab/ref/serialport.html  



