# Lab_1_instrumentacion
### Integrantes:
Lina María Cortes Almonacid  
Karen Dayanna Mora Segura  
Sofia Alejandra Cardona Cruz     

En la presente práctica de laboratorio se realizó el monitoreo del proceso respiratorio mediante la medición de la concentración de dióxido de carbono (CO₂) en el aire exhalado, utilizando un sistema de adquisición de datos basado en una placa ESP32 y un sensor de CO₂. Este sistema permitió capturar las variaciones de la señal respiratoria y visualizarla inicialmente en el entorno de Arduino IDE, para posteriormente procesar los datos en Python con el fin de graficar y analizar su comportamiento bajo diferentes condiciones respiratorias. A partir de este análisis fue posible identificar el patrón respiratorio, estimar la frecuencia respiratoria y observar sus variaciones según la actividad del sujeto, comprendiendo así el funcionamiento básico de un sistema de adquisición de señales biomédicas y la relación entre la concentración de CO₂ y el proceso fisiológico de la respiración.  
### Objetivo General  
Evaluar la influencia del habla sobre el patrón y la frecuencia respiratoria mediante el desarrollo de un sistema de adquisición de datos utilizando una ESP32 y un sensor de CO₂, así como el procesamiento de la señal obtenida para su posterior análisis.  


### Objetivos específicos  
* Reconocer las principales variables físicas involucradas en el proceso respiratorio.  
* Implementar un sistema de adquisición de datos con una ESP32 y un sensor de CO₂ para capturar la señal respiratoria.  
* Procesar y analizar la señal adquirida en Python para obtener el patrón y la frecuencia respiratoria.  
* Identificar los cambios que se presentan en el patrón respiratorio cuando el sujeto se encuentra en reposo y cuando está hablando.  

### Metodología  
Para el desarrollo de esta práctica se implementó un sistema de adquisición de la señal respiratoria utilizando una placa ESP32 y un sensor de dióxido de carbono (CO₂). Este tipo de sistema permite registrar las variaciones en la concentración de CO₂ presentes en el aire exhalado durante la respiración, las cuales pueden relacionarse con el patrón respiratorio y utilizarse para estimar la frecuencia respiratoria [1].  

El principio de funcionamiento del sistema se basa en que el aire exhalado contiene una concentración de dióxido de carbono considerablemente mayor que el aire ambiente. Debido a esto, cuando una persona exhala cerca del sensor, este detecta el incremento en la concentración de CO₂ y genera una variación en su señal de salida. Durante la inhalación ocurre el efecto contrario, ya que disminuye la cantidad de CO₂ detectada, permitiendo obtener una señal que representa los ciclos respiratorios [2].  

Antes de iniciar la adquisición de datos, se realizó el montaje del sistema conectando el sensor de CO₂ a la placa ESP32 de acuerdo con las especificaciones del fabricante y verificando que todas las conexiones eléctricas fueran correctas. Posteriormente, el sensor se ubicó a una distancia corta de la nariz y la boca del sujeto de prueba, procurando que el aire exhalado llegara directamente al elemento sensor sin necesidad de contacto físico, lo que permitió registrar las variaciones producidas durante cada respiración [2].  

Con el montaje terminado, el sujeto permaneció en reposo para reducir movimientos que pudieran afectar la medición. Durante la práctica se realizaron mediciones tanto en condición de reposo como durante una actividad de verbalización, permitiendo obtener señales respiratorias bajo diferentes condiciones fisiológicas para su posterior análisis. Estas mediciones se realizaron manteniendo una distancia similar entre el sensor y el sujeto durante toda la adquisición, con el fin de disminuir posibles variaciones asociadas a la posición del sensor y mejorar la consistencia de los datos registrados [1], [2].  


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
[1] Hall, J. E. (2021). Guyton y Hall. Tratado de fisiología médica (14.ª ed.). Elsevier. https://www.elsevier.com/books/guyton-and-hall-textbook-of-medical-physiology/hall/978-0-323-59712-8. 

[2] Universidad Militar Nueva Granada. (2025). Guía de práctica de laboratorio: Monitoreo del patrón y frecuencia respiratoria.   

[3] Webster, J. G., & Clark, J. W. (2010). Medical Instrumentation: Application and Design (4th ed.). John Wiley & Sons. https://www.wiley.com/en-us/Medical+Instrumentation:+Application+and+Design,+4th+Edition-p-9780471676003. 
