# Laboratorio-3-Fotopletismografia(PPG)
- María Angel Benavides Silva - 5600852
- Evelyn Marcela Caro Rodríguez - 5600848
## PARTE A 
Para comenzar el desarrollo del presente laboratorio se construye el circuito que se muestra a continuación, empleando el sensor TCST110

<img width="1102" height="605" alt="image" src="https://github.com/user-attachments/assets/4359f1af-b112-4b88-8aa3-d6d5866c906e" />

Aplicando un voltaje de 3 Voltios, y modificando el sensor, con el fin de poder ubicar la huella del dedo sobre el sensor y poder de esta manera capturar la señal de fotopletismografía. 

Para comenzar probamos la primera seción del circuito en la que se encuenta el sensor. Quedando de la siguiemte manera:

<img width="1536" height="1152" alt="WhatsApp Image 2026-09-23 at 1 34 13 PM" src="https://github.com/user-attachments/assets/5a346866-01dc-4e13-bbc1-5a1c53b47776" />

Sin embargo, tras varios intentos, no se logró el funcionamiento correcto del sensor, lo cual se debe a una posible conexión errónea del circuito  o un fallo en el sensor, por lo que posteriormente se implementa un nuevo sensor (MAX1030) el cual se conecta mediante Arduino y se logra de esta manera obtener la señal de fotopletismografía (PPG) quedando conectado como se muestra en la siguiente figura:

<img width="591" height="1280" alt="WhatsApp Image 2026-09-23 at 1 43 01 PM" src="https://github.com/user-attachments/assets/df0b01e4-e3be-4208-8258-9b1eda8b543f" />

##

### Técnica “Cold Pressor Test” (CPT)

La técnica "Cold Pressor Test es un procedimiento experimental económico, fiable y válido que consiste en sumergir la mano y el antebrazo en un recipiente de agua circulante a una temperatura aproximada de 0 °C. A esta temperatura, la estimulación induce respuestas del sistema nervioso simpático, tales como vasoconstricción arterial, aumento de la presión sanguínea y disminución del flujo sanguíneo, lo que genera una sensación dolorosa progresiva pero controlada, antes de la prueba se realiza un cuestionario de salud para descartar contraindicaciones como el síndrome de Raynaud, patologías cardiocirculatorias, antecedentes de dolor crónico, diabetes, epilepsia, lesiones recientes o uso de medicación [1].

Los principales parámetros que se miden con este test son: umbral, mantenimiento y la tolerancia al dolor, con el fin de identificar el tiempo máximo que puede soportar el paciente en cada parámetro, tomando el tiempo en segundos con un límite de 4-5 minutos para evitar posibles afecciones y garantizar la seguridad del paciente. Este test se utiliza para poder realizar pruebas en laboratorios para evaluar componentes psicológicos antes de aplicarlos en el ámbito médico como por ejemplo medir y comparar de forma cuantitativa la eficacia de la analgesia hipnótica frente a técnicas de distracción simple, estrategias cognitivas (como imágenes guiadas), entrenamiento autógeno, placebo y fármacos [1].

Los hallazgos validados mediante el CPT se transfieren directamente a la intervención clínica con pacientes como por ejemplo:

- **Procedimientos médicos agudos e invasivos:** Se aplican técnicas hipnóticas y cognitivas para reducir el dolor en extracciones de médula ósea (especialmente en oncología pediátrica y adulta), curas de quemaduras, reducción de fracturas sin anestesia e intervenciones odontológicas.
- **Reducción de medicación analgésica:** En cuadros de dolor crónico y en pacientes terminales de cáncer, el uso de estas técnicas permite disminuir la dependencia de fármacos analgésicos pesados, ayudando a los pacientes a conservar la lucidez en sus etapas finales.
- **Deporte de élite y profesiones de alto riesgo:** El CPT se utiliza además para evaluar y entrenar la capacidad de resistencia física y mental, así como la tolerancia al dolor, en atletas de alto rendimiento y en profesiones de exigencia extrema [1].


## PARTE B
SPI
##
### Código MATLAB y Resultados obtenidos

Se obtuvieron 2 resultados uno de una integrante del grupo y otros obtenidos con el docente.

1. Integrante del grupo
   
Señal PPG:

<img width="1600" height="680" alt="image" src="https://github.com/user-attachments/assets/6ff531db-6aec-4bbb-af08-77bfdde28117" />

 Gráfico SPI

<img width="1600" height="748" alt="image" src="https://github.com/user-attachments/assets/3972e110-2989-41af-b42e-347407b438b0" />

Índice SPI

<img width="441" height="201" alt="image" src="https://github.com/user-attachments/assets/c2710e83-642e-459c-867d-80c856ca278f" />

2. Datos obtenidos con el Docente

Señal PPG 

   <img width="1600" height="731" alt="image" src="https://github.com/user-attachments/assets/b4a00cab-1cbf-4ffa-a2dc-87df15863828" />

Gráfico SPI

<img width="1600" height="740" alt="image" src="https://github.com/user-attachments/assets/72e5150c-c889-48f6-8b6b-27e76e847dde" />

Índice SPI

<img width="622" height="267" alt="image" src="https://github.com/user-attachments/assets/ca883861-3ec4-46a0-9497-08e670619235" />


## CONCLUSION

## REFERENCIAS
[1] J. M. Carrillo, S. Collado Vázquez y N. Rojo, “El Cold Pressor Test en la investigación del dolor experimental y clínico,” Biociencias, vol. 3, Universidad Alfonso X el Sabio, Madrid, 2005.

[2] 

[3] E. J. Argüello-Prada, “The mountaineer’s method for peak detection in photoplethysmographic signals,” Revista Facultad de Ingeniería, Universidad de Antioquia, no. 90, pp. 42–50, Jan.–Mar. 2019, doi:10.17533/udea.redin.n90a06.
