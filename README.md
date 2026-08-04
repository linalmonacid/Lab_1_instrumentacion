# Laboratorio 1: Monitoreo del patrón y frecuencia respiratoria
### Integrantes:
Lina María Cortes Almonacid  
Karen Dayanna Mora Segura  
Sofia Alejandra Cardona Cruz     

En la presente práctica de laboratorio se realizó el monitoreo del proceso respiratorio mediante la adquisición de las variaciones de sonido producidas durante la respiración y el habla, utilizando un sistema de adquisición de datos basado en una placa ESP32 y un micrófono KY-038. Durante el ciclo respiratorio, la entrada y salida de aire generan pequeñas variaciones acústicas que pueden ser detectadas por el micrófono, mientras que al hablar se producen ondas sonoras de mayor amplitud y frecuencia debido a la vibración de las cuerdas vocales. Estas diferencias permiten distinguir entre los distintos estados respiratorios y el habla, convirtiendo al micrófono en un sensor adecuado para registrar el comportamiento de la señal respiratoria. Con este sistema se capturaron las variaciones de la señal, las cuales se visualizaron inicialmente en el entorno de Arduino IDE, para posteriormente procesar los datos en Python con el fin de graficar y analizar su comportamiento bajo diferentes condiciones respiratorias. A partir de este análisis fue posible identificar el patrón respiratorio, estimar la frecuencia respiratoria y observar sus variaciones según la actividad del sujeto, comprendiendo así el funcionamiento básico de un sistema de adquisición de señales biomédicas y la relación entre las variaciones acústicas y el proceso fisiológico de la respiración.  
### Objetivo General  
Evaluar la influencia del habla sobre el patrón y la frecuencia respiratoria mediante el desarrollo de un sistema de adquisición de datos utilizando una ESP32 y un micrófono KY-038, así como el procesamiento de la señal obtenida para su posterior análisis.   


### Objetivos específicos  
* Reconocer las principales características acústicas presentes durante la respiración y el habla.  
* Implementar un sistema de adquisición de datos con una ESP32 y un micrófono KY-038 para capturar las variaciones de sonido generadas por el sujeto.  
* Procesar y analizar la señal adquirida en Python para obtener el patrón y la frecuencia respiratoria.  
* Identificar los cambios que se presentan en la señal cuando el sujeto se encuentra en reposo y cuando está hablando.   
### Metodología  
Para el desarrollo de esta práctica se implementó un sistema de adquisición de la señal respiratoria utilizando una placa ESP32 y un micrófono KY-038. Este sistema permite registrar las variaciones de intensidad del sonido producidas durante la respiración y el habla, las cuales pueden emplearse para identificar el patrón respiratorio y estimar la frecuencia respiratoria mediante el procesamiento de la señal [1].  

El principio de funcionamiento del sistema se basa en que el micrófono KY-038 convierte las ondas sonoras en una señal eléctrica analógica. Durante la respiración se generan sonidos de baja intensidad asociados al flujo de aire durante la inhalación y la exhalación, mientras que al hablar se producen señales de mayor amplitud y contenido frecuencial debido a la vibración de las cuerdas vocales. Estas diferencias permiten distinguir ambos estados mediante el análisis de la señal registrada [2].

Antes de iniciar la adquisición de datos, se realizó el montaje del sistema conectando el micrófono KY-038 a la placa ESP32 de acuerdo con las especificaciones del módulo y verificando que todas las conexiones eléctricas fueran correctas. Posteriormente, el micrófono se ubicó a una distancia corta de la nariz y la boca del sujeto de prueba, procurando captar con claridad los sonidos generados durante la respiración y el habla sin que existiera contacto físico entre el sensor y el sujeto. Esta disposición permitió registrar las variaciones acústicas producidas durante cada ciclo respiratorio y durante la verbalización [2].  

Con el montaje terminado, el sujeto permaneció inicialmente en reposo para reducir movimientos que pudieran afectar la adquisición de la señal. Posteriormente, se realizaron mediciones tanto en condición de reposo como durante una actividad de verbalización, permitiendo obtener registros bajo diferentes condiciones para su posterior análisis. Durante todas las pruebas se mantuvo una distancia similar entre el micrófono y el sujeto, con el fin de reducir variaciones en la intensidad de la señal debidas a cambios de posición y mejorar la consistencia de los datos registrados [1], [2].  
### Discusión   
1. ¿Son los patrones respiratorios y las frecuencias respiratorias iguales o diferentes en cada caso? ¿A qué se debe esto?

No, los patrones respiratorios y la frecuencia respiratoria no fueron iguales en las dos condiciones evaluadas (reposo y verbalización). Durante el reposo se observó una respiración más constante y periódica, con ciclos de inhalación y exhalación relativamente uniformes. En cambio, durante la verbalización el patrón respiratorio presentó cambios debido a que el sujeto debía coordinar la respiración con el habla, generando exhalaciones más prolongadas e inhalaciones más cortas y menos frecuentes [1].  


Estas diferencias se deben a que la respiración no solo cumple la función de realizar el intercambio de gases, sino que también participa en la producción de la voz. Cuando una persona habla, el sistema respiratorio adapta el ritmo respiratorio para suministrar el flujo de aire necesario durante la fonación, modificando temporalmente la frecuencia y el patrón normal de respiración [2]. Por esta razón, es esperado que las señales respiratorias obtenidas durante la verbalización presenten un comportamiento diferente al registrado en reposo.  

2. ¿Cuáles serían las ventajas y desventajas de emplear múltiples sensores para el monitoreo del proceso respiratorio? ¿Cuáles podrían ser las razones?

El uso de múltiples sensores puede mejorar la calidad del monitoreo respiratorio, ya que permite obtener información desde diferentes variables fisiológicas. Por ejemplo, un sensor de CO₂ puede complementarse con sensores de flujo de aire, temperatura o movimiento torácico para obtener una descripción más completa del proceso respiratorio y aumentar la confiabilidad de las mediciones [3].  

Sin embargo, utilizar varios sensores también presenta algunas desventajas. En primer lugar, aumenta el costo del sistema y la complejidad del montaje. Además, es necesario sincronizar correctamente las señales obtenidas por cada sensor para evitar errores durante el análisis. También puede resultar menos cómodo para el usuario si se requiere colocar varios dispositivos sobre el cuerpo, lo que podría afectar la naturalidad de la respiración y, por lo tanto, influir en los resultados obtenidos [2], [3].  

En general, la elección entre utilizar uno o varios sensores dependerá de la aplicación. Para un monitoreo básico de la frecuencia respiratoria, un solo sensor puede ser suficiente; mientras que para aplicaciones clínicas o de investigación, el uso de múltiples sensores permite obtener información más completa y reducir la posibilidad de errores en la interpretación de la señal.  

### Conclusiones  

### Referencias  

[1] Sauter, T. (2021). The ESP32: A powerful IoT microcontroller. Springer. https://link.springer.com/book/10.1007/978-1-4842-6717-7. 

[2] Joy-IT. (s. f.). KY-038 Microphone Sensor Module – Technical Manual. https://joy-it.net/files/files/Produkte/KY-038/KY-038_Manual_2020-09-16.pdf.  

[3] Universidad Militar Nueva Granada. (2025). Guía de práctica de laboratorio: Monitoreo del patrón y frecuencia respiratoria.   


