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

En la señal registrada durante la respiración en reposo se observaron variaciones suaves, con aumentos y descensos que pueden relacionarse con los ciclos de inhalación y exhalación. Aunque la forma de la señal no fue completamente periódica ni presentó una onda tan definida como la esperada, sí fue posible reconocer varios cambios repetitivos a lo largo del registro. Esto indica que el sistema logró detectar parte de las variaciones acústicas generadas por la respiración.  

<img width="881" height="286" alt="image" src="https://github.com/user-attachments/assets/742225a7-b4a8-4e3f-85c2-d00030026cff" />  


Al aplicar la Transformada Rápida de Fourier a la señal de reposo, se obtuvo una frecuencia dominante de 0,200 Hz, equivalente a una frecuencia respiratoria de 12 respiraciones por minuto. Este resultado es coherente con lo observado en la gráfica temporal, ya que se distinguen varios ciclos respiratorios durante el intervalo analizado. Por lo tanto, puede considerarse que el sistema fue capaz de estimar una frecuencia respiratoria razonable en esta condición.  
<img width="1056" height="340" alt="image" src="https://github.com/user-attachments/assets/36b1874f-a889-4b94-b018-2dbc220f2285" />  

Sin embargo, la señal también presentó variaciones irregulares y cambios de amplitud que no corresponden claramente a un ciclo respiratorio. Estas alteraciones pudieron deberse al ruido del ambiente, al movimiento del tapabocas, al roce del micrófono con la tela y a pequeñas modificaciones en la posición del sensor con respecto a la nariz y la boca. Como el micrófono registraba sonidos y no directamente el flujo de aire, cualquier sonido cercano podía mezclarse con la respiración y afectar la forma final de la señal.  

Otro factor importante fue el estado físico del micrófono. Durante la práctica se observó que una de las conexiones soldadas se encontraba parcialmente suelta. Esto pudo producir un contacto eléctrico inestable y provocar cambios repentinos en las mediciones, pérdida momentánea de información o valores de amplitud diferentes aun cuando la respiración de la persona no hubiera cambiado. Esta condición puede explicar por qué algunos ciclos aparecen más marcados que otros y por qué la gráfica no presenta una forma uniforme.  

A pesar de estas limitaciones, el resultado obtenido en reposo fue útil, ya que permitió identificar una frecuencia dominante clara y reconocer un comportamiento relativamente repetitivo. Por esta razón, se puede afirmar que el sistema funcionó de manera parcial: logró detectar el patrón respiratorio general, aunque la calidad de la señal estuvo limitada por el ruido, la ubicación del sensor y la conexión inestable del micrófono.  

Luego se obtuvo la señaldurante el habla donde se observó un comportamiento más irregular que en reposo, la amplitud presentó aumentos y descensos de diferente duración, sin conservar un patrón claramente periódico. Este resultado era esperado en cierta medida, ya que al hablar la persona modifica su respiración para poder pronunciar palabras y frases.

<img width="1014" height="345" alt="image" src="https://github.com/user-attachments/assets/021b9385-45ce-4ab5-ab3e-674272c7cbd0" />  

Además, en esta condición el micrófono recibió al mismo tiempo los sonidos de la respiración y los sonidos producidos por el habla. Como la voz suele tener una intensidad mayor que los sonidos respiratorios, pudo dominar la medición y dificultar la identificación precisa de las inhalaciones y exhalaciones. Por esta razón, la señal obtenida representa una combinación entre respiración, voz, movimientos del tapabocas y ruido del entorno, y no exclusivamente el patrón respiratorio.  

<img width="1048" height="341" alt="image" src="https://github.com/user-attachments/assets/37b76e0a-7033-4fe8-8d0b-142ca6bb21a1" />  

En el análisis de frecuencia se obtuvo una frecuencia dominante de 0,125 Hz, equivalente a 7,5 respiraciones por minuto. Este valor fue menor que el encontrado en reposo. Es posible que durante el habla la persona realizó inhalaciones más separadas y exhalaciones más prolongadas para completar palabras o frases. En ese caso, se presentarían menos ciclos respiratorios completos durante el mismo intervalo de medición.

El problema de la conexión parcialmente suelta también pudo afectar con mayor intensidad esta prueba, durante el habla se producen pequeños movimientos en el tapabocas, la mandíbula y el rostro, los cuales pudieron mover el micrófono o la zona soldada. Si el contacto eléctrico cambiaba durante estos movimientos, la señal podía presentar aumentos o caídas que no correspondían realmente a cambios respiratorios. También la calibración inicial también pudo verse afectada por estas interferencias. Durante esta etapa, el sistema calculó un nivel de referencia mientras la persona permanecía en silencio, pero usualmente había ruido ambiental, movimiento del tapabocas o un contacto irregular en la soldadura, el valor de referencia pudo quedar desplazado. Como las lecturas posteriores fueron comparadas con ese valor, una calibración inadecuada pudo causar que la señal respiratoria apareciera desplazada, con amplitudes mayores o menores de las esperadas.  

A pesar de estos inconvenientes, la señal durante el habla sí mostró un comportamiento diferente al observado en reposo. Se identificaron cambios más abruptos, amplitudes variables y una menor regularidad, lo cual permite evidenciar que la verbalización modifica el patrón registrado por el sistema.  

========== RESULTADOS ==========  
REPOSO   
Frecuencia dominante: 0.200 Hz   
Frecuencia respiratoria: 12.00 RPM  
HABLANDO   
Frecuencia dominante: 0.125 Hz  
Frecuencia respiratoria: 7.50 RPM   
Diferencia: -4.50 RPM

Los resultados permitieron observar una diferencia entre ambas condiciones. En reposo se registró una mayor frecuencia respiratoria y un patrón relativamente más regular, mientras que durante el habla se obtuvo una frecuencia menor y una señal más irregular. A pesar de que las gráficas no presentaron exactamente las formas esperadas, el sistema permitió reconocer cambios relacionados con la verbalización.  

### Ventajas
El sistema desarrollado tiene como principales ventajas su bajo costo, facilidad de implementación y portabilidad, debido a que utiliza un micrófono KY, una ESP32 y herramientas de fácil acceso como Arduino IDE y MATLAB. Además, permite adquirir y visualizar la señal en tiempo real, guardar los datos para analizarlos posteriormente y comparar el comportamiento respiratorio en reposo y durante el habla. El uso de un micrófono dentro del tapabocas también resulta poco invasivo y permite detectar los sonidos producidos por la inhalación y la exhalación sin colocar sensores directamente sobre el tórax. Estudios similares han demostrado que los micrófonos instalados en mascarillas pueden utilizarse para estimar la frecuencia respiratoria a partir de los sonidos de la respiración.  

### Limitaciones  
La principal limitación es que el micrófono no mide directamente el flujo de aire ni el movimiento del tórax, sino los sonidos relacionados con la respiración. Por esta razón, la señal puede verse afectada por la voz, el ruido ambiental, el roce del tapabocas y los movimientos de la persona. En esta práctica también influyeron la conexión soldada parcialmente suelta y el ruido presente durante la calibración, lo que pudo generar lecturas inestables y modificar la frecuencia calculada.  

Asimismo, los filtros y umbrales fueron ajustados experimentalmente para una sola persona, por lo que el sistema no puede considerarse un equipo clínico ni utilizarse para diagnosticar patologías sin realizar pruebas con más participantes y compararlo con un instrumento de referencia. Los sistemas acústicos requieren una buena relación entre la señal respiratoria y el ruido para lograr estimaciones confiables, mientras que la combinación con otros sensores puede mejorar la estabilidad y la identificación del patrón respiratorio.


### Discusión   
1. ¿Son los patrones respiratorios y las frecuencias respiratorias iguales o diferentes en cada caso? ¿A qué se debe esto?

Los patrones y las frecuencias respiratorias fueron diferentes en cada condición. En reposo se obtuvo un patrón relativamente regular y una frecuencia de 12 respiraciones por minuto, mientras que durante el habla la señal fue más irregular y la frecuencia disminuyó a 7,5 respiraciones por minuto. Fisiológicamente, esto ocurre porque al hablar la respiración se adapta a la producción de la voz: la persona suele inhalar antes de comenzar una frase y después realiza una exhalación más larga y controlada para pronunciar las palabras. Además, la duración de las frases, las pausas y la intensidad de la voz modifican el momento y la profundidad de las inhalaciones [8]. En los resultados también pudieron influir el ruido ambiental, la calibración y la conexión parcialmente suelta del micrófono.  


2. ¿Cuáles serían las ventajas y desventajas de emplear múltiples sensores para el monitoreo del proceso respiratorio? ¿Cuáles podrían ser las razones?

El uso de múltiples sensores tendría como ventaja obtener información complementaria y más confiable del proceso respiratorio. Por ejemplo, el micrófono podría captar los sonidos, una banda torácica medir el movimiento del pecho y un sensor de flujo detectar el aire inhalado y exhalado. La combinación de estas señales permitiría confirmar cada respiración, diferenciar mejor la voz del movimiento respiratorio y mantener la medición cuando uno de los sensores presente ruido o una falla [9]. 

Sin embargo, utilizar varios sensores también presenta algunas desventajas. En primer lugar, aumenta el costo del sistema y la complejidad del montaje. Además, es necesario sincronizar correctamente las señales obtenidas por cada sensor para evitar errores durante el análisis. También puede resultar menos cómodo para el usuario si se requiere colocar varios dispositivos sobre el cuerpo, lo que podría afectar la naturalidad de la respiración y, por lo tanto, influir en los resultados obtenidos.  

En general, la elección entre utilizar uno o varios sensores dependerá de la aplicación. Para un monitoreo básico de la frecuencia respiratoria, un solo sensor puede ser suficiente; mientras que para aplicaciones clínicas o de investigación, el uso de múltiples sensores permite obtener información más completa y reducir la posibilidad de errores en la interpretación de la señal.  

### Conclusiones  
- Se logró desarrollar un sistema capaz de adquirir, procesar y representar señales relacionadas con la respiración mediante un micrófono, una ESP32 y MATLAB. Aunque las gráficas no presentaron completamente la forma esperada, fue posible identificar cambios en el patrón respiratorio y obtener una frecuencia dominante para cada condición evaluada.
- EL habla modificó el comportamiento respiratorio del sujeto, en reposo se obtuvo una frecuencia de 12 respiraciones por minuto, mientras que durante el habla se registraron 7,5 respiraciones por minuto. Esta disminución puede explicarse porque, al hablar, la persona realiza inhalaciones más separadas y exhalaciones más largas y controladas para producir palabras y frases.
- El procesamiento mediante eliminación del componente DC, cálculo RMS y filtrado pasa-bajas permitió suavizar la señal y facilitar la identificación de sus variaciones. Sin embargo, la calidad de los resultados dependió considerablemente de la correcta calibración, la ubicación del micrófono y las condiciones del ambiente.
- El ruido externo, el movimiento y roce del tapabocas, así como la conexión soldada parcialmente suelta del micrófono, afectaron la estabilidad de las mediciones. Por esta razón, las frecuencias calculadas deben considerarse valores aproximados y no mediciones clínicas exactas.
- Los sonidos respiratorios pueden utilizarse para realizar un monitoreo básico y de bajo costo, pero no representan directamente el flujo de aire ni el movimiento del tórax. Para detectar posibles anomalías respiratorias con mayor confiabilidad sería conveniente complementar el micrófono con sensores de flujo, presión o expansión torácica, además de realizar pruebas con más personas y comparar los resultados con un equipo de referencia.  

### Referencias  
[1] Universidad Militar Nueva Granada. (2025). Guía de práctica de laboratorio: Monitoreo del patrón y frecuencia respiratoria.   
[2] Romano, C., Nicolò, A., Innocenti, L., Bravi, M., Miccinilli, S., Sterzi, S., Sacchetti, M., Schena, E., & Massaroni, C. (2023). Respiratory rate estimation during walking and running using breathing sounds recorded with a microphone. Biosensors, 13(6), 637. https://doi.org/10.3390/bios13060637  

[3] Fuchs, S., & Rochet-Capellan, A. (2021). The respiratory foundations of spoken language. Annual Review of Linguistics, 7, 13–30. https://doi.org/10.1146/annurev-linguistics-031720-103907  

[4] Espressif Systems. (s. f.). Analog to digital converter (ADC)—ESP32 programming guide. https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/peripherals/adc/index.html  

[5] Microchip Technology Inc. (2022). Processing analog sensor data with digital filtering (Application Note AN4515). https://www.microchip.com/en-us/application-notes/an4515  

[6] MathWorks. (s. f.). rms: Root mean square value. https://www.mathworks.com/help/matlab/ref/rms.html  

[7] MathWorks. (s. f.). serialport: Connection to serial port. https://www.mathworks.com/help/matlab/ref/serialport.html  

[8] Binazzi, B., Lanini, B., Bianchi, R., Romagnoli, I., Nerini, M., Gigliotti, F., & Scano, G. (2006). Breathing pattern and kinematics in normal subjects during speech, singing and loud whispering. Acta Physiologica, 186(3), 233–246. https://pubmed.ncbi.nlm.nih.gov/16497202/

[9] Moon, K. S., Choi, H., & Lee, S. (2023). A wearable multimodal wireless sensing system for respiratory pattern monitoring. Sensors, 23. https://pmc.ncbi.nlm.nih.gov/articles/PMC10422350/



