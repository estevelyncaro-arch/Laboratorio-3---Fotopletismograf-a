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

<img width="1280" height="591" alt="WhatsApp Image 2026-09-24 at 6 27 37 PM" src="https://github.com/user-attachments/assets/1c461ef1-a9dc-4845-9c77-373dabfd2f23" />


##

### Técnica “Cold Pressor Test” (CPT)

La técnica "Cold Pressor Test" es un procedimiento experimental económico, fiable y válido que consiste en sumergir la mano y el antebrazo en un recipiente de agua circulante a una temperatura aproximada de 0 °C. A esta temperatura, la estimulación induce respuestas del sistema nervioso simpático, tales como vasoconstricción arterial, aumento de la presión sanguínea y disminución del flujo sanguíneo, lo que genera una sensación dolorosa progresiva pero controlada, antes de la prueba se realiza un cuestionario de salud para descartar contraindicaciones como el síndrome de Raynaud, patologías cardiocirculatorias, antecedentes de dolor crónico, diabetes, epilepsia, lesiones recientes o uso de medicación [1].

Los principales parámetros que se miden con este test son: umbral, mantenimiento y la tolerancia al dolor, con el fin de identificar el tiempo máximo que puede soportar el paciente en cada parámetro, tomando el tiempo en segundos con un límite de 4-5 minutos para evitar posibles afecciones y garantizar la seguridad del paciente. Este test se utiliza para poder realizar pruebas en laboratorios para evaluar componentes psicológicos antes de aplicarlos en el ámbito médico como por ejemplo medir y comparar de forma cuantitativa la eficacia de la analgesia hipnótica frente a técnicas de distracción simple, estrategias cognitivas (como imágenes guiadas), entrenamiento autógeno, placebo y fármacos [1].

Los hallazgos validados mediante el CPT se transfieren directamente a la intervención clínica con pacientes como por ejemplo:

- **Procedimientos médicos agudos e invasivos:** Se aplican técnicas hipnóticas y cognitivas para reducir el dolor en extracciones de médula ósea (especialmente en oncología pediátrica y adulta), curas de quemaduras, reducción de fracturas sin anestesia e intervenciones odontológicas.
- **Reducción de medicación analgésica:** En cuadros de dolor crónico y en pacientes terminales de cáncer, el uso de estas técnicas permite disminuir la dependencia de fármacos analgésicos pesados, ayudando a los pacientes a conservar la lucidez en sus etapas finales.
- **Deporte de élite y profesiones de alto riesgo:** El CPT se utiliza además para evaluar y entrenar la capacidad de resistencia física y mental, así como la tolerancia al dolor, en atletas de alto rendimiento y en profesiones de exigencia extrema [1].


## PARTE B
### SPI
 El Índice Pletismográfico Quirúrgico (SPI) es una variable numérica estandarizada en una escala de 0 a 100, desarrollada para evaluar de forma objetiva el nivel de estrés quirúrgico y nociceptivo en pacientes bajo anestesia general. Su propósito principal es cuantificar el equilibrio dinámico entre la intensidad de los estímulos nocivos derivantes de la cirugía (nocicepción) y la cobertura que proporcionan los fármacos analgésicos u opioides (antinocicepción), lo que permite detectar la activación del sistema nervioso simpático provocada por la estimulación dolorosa.
 El modelo se compone del análisis continuo de dos señales cardiovasculares no invasivas extraídas durante la monitorización: la amplitud de la onda de pulso fotopletismográfica (PPG), obtenida mediante el oxímetro de pulso en el dedo, y el intervalo entre latidos cardíacos (HBI), derivado del electrocardiograma o de la onda de pulso. La PPG refleja la vasoconstricción periférica provocada por el tono simpático (disminuyendo ante estímulos dolorosos), mientras que el HBI se acorta a medida que aumenta la frecuencia cardíaca [2].

El modelo matemático final del índice se derivó mediante un ajuste de mínimos cuadrados utilizando como referencia una estimación calculada del Estrés Quirúrgico Total (TSS), la cual correlacionaba la intensidad de estímulos dolorosos reales (como la intubación o la incisión) con las concentraciones de remifentanilo en el sitio. 

La formulación matemática exacta del índice se expresa mediante la siguiente combinación lineal:
SPI = 100 - (0.7 * PPGA_norm + 0.3*HBI_norm)
En esta ecuación, el parámetro vascular PPGA_norm recibe una ponderación relativa del 70% (0.7), mientras que el parámetro cronotrópico (HBI_norm) representa el 30% restante (0.3), lo que refleja que la amplitud del pulso es la variable individual con mejor correlación tanto con la severidad del estímulo nocivo como con la concentración del opioide.En la interpretación del índice, un valor cercano a 100 indica un nivel de estrés quirúrgico muy elevado o una analgesia insuficiente, un valor cercano a 0 representa un estado de baja nocicepción o analgesia profunda, y el valor 50 corresponde al nivel medio de estrés [2]. 

##
### Código MATLAB y Resultados obtenidos

Para este laboratio se realizo un codigo que capturarar y se pudieran evidenciar la grafica de PPG (Fotoplestimografia), para luego utilizar el metodo del alpinista para detectar los picos y los valles para la deteccion de indice plestimografico teniendo un una fase basal, una en donde se hizo la prueba de CPT y en una donde hay una recuperación 

```matlab
% =========================================================================
% CAPTURA PPG EN TIEMPO REAL CON TIEMPO CRONOMETRADO Y POST-PROCESAMIENTO
% =========================================================================

clear; clc; close all;

puertoCOM = 'COM7';      
baudRate = 115200;     

% Verifica si el puerto COM7 quedó abierto en una ejecución previa y lo libera
if ~isempty(serialportfind("Port", puertoCOM))
    delete(serialportfind("Port", puertoCOM));
end

disp('Conectando a Arduino en COM7...');
try
    % Abre la comunicación serial con un tiempo de espera (Timeout) de 1 segundo
    s = serialport(puertoCOM, baudRate, "Timeout", 1);
    
    % Define 'LF' (Line Feed / \n) como el carácter final de cada línea enviada por Arduino
    configureTerminator(s, "LF");
    
    % Limpia cualquier dato antiguo o residual presente en el búfer del puerto
    flush(s);
    disp('Conexión exitosa.');
catch ME
    error('No se pudo conectar al puerto COM7.');
end
```

En la primera parte del código se realiza la lectura de la placa Arduino UNO, definiendo el canal de entrada por el cual se recibe la señal fisiológica. Este paso inicial permite establecer la conexión entre el hardware y el software, garantizando que la señal capturada pueda ser procesada y posteriormente representada para el análisis correspondiente.

```matlab
import numpy as np
from scipy.signal import butter, lfilter

# --- 1. Definición de Filtro Digital (Butterworth Bandpass) ---
def butter_bandpass(lowcut, highcut, fs, order=4):
    nyq = 0.5 * fs
    low = lowcut / nyq
    high = highcut / nyq
    b, a = butter(order, [low, high], btype='band')
    return b, a

def filtrar_senal(data, lowcut=0.5, highcut=45.0, fs=500.0):
    """
    Aplica un filtro pasa-banda para limpiar ruido de alta frecuencia
    y la deriva de línea base en señales fisiológicas.
    """
    b, a = butter_bandpass(lowcut, highcut, fs, order=2)
    y = lfilter(b, a, data)
    return y

# --- 2. Bucle de Procesamiento y Representación ---
# Supongamos que 'buffer_datos' almacena la ventana de tiempo a graficar
FS = 500  # Frecuencia de muestreo en Hz

def procesar_y_representar(buffer_datos):
    if len(buffer_datos) < FS:
        return  # Esperar a tener suficientes muestras
    
    # Conversión de lectura analógica (0 - 1023) a Voltaje (0 - 5V)
    voltaje = [(muestra * 5.0) / 1023.0 for muestra in buffer_datos]
    
    # Filtrado de la señal
    senal_filtrada = filtrar_senal(voltaje, lowcut=0.5, highcut=45.0, fs=FS)
    
    # Representación/Actualización de datos para la gráfica
    return voltaje, senal_filtrada
```

En la siguiente sección del código se implementan filtros digitales que permiten eliminar tanto el ruido de alta frecuencia como el componente DC derivado de la línea base, mediante la aplicación de un filtro pasa banda. Además, se incorpora un buffer circular, cuya función es organizar las muestras procesadas dentro de una ventana temporal, lo que facilita la visualización y el análisis continuo de la señal a lo largo de la adquisición de datos.

```matlab
```


En donde se obtuvieron 2 resultados uno de una integrante del grupo y otros obtenidos con el docente.

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

[2] M. Huiku et al., “Assessment of surgical stress during general anaesthesia,” British Journal of Anaesthesia, vol. 98, no. 4, pp. 447–455, 2007, doi: 10.1093/bja/aem004.

[3] E. J. Argüello-Prada, “The mountaineer’s method for peak detection in photoplethysmographic signals,” Revista Facultad de Ingeniería, Universidad de Antioquia, no. 90, pp. 42–50, Jan.–Mar. 2019, doi:10.17533/udea.redin.n90a06.
