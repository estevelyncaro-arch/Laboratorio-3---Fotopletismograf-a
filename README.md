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
if isempty(time_log)
    error('No se recibieron datos del puerto COM7 durante la captura.');
end

num_upsteps = 0; 
threshold = 2;

time_history = [];
spi_history  = [];

last_peak_time  = 0; 
last_peak_val   = 0;
last_valley_val = 0;

ibi_current  = 0.8;
ppga_current = 1000;

peaks_time = []; peaks_val = [];
valleys_time = []; valleys_val = [];
proc_buffer = [];

N = length(clean_ppg_log);

for k = 1:N
    valor_muestra = clean_ppg_log(k);
    t_sample      = time_log(k);
    
    proc_buffer(end+1) = valor_muestra;
    if length(proc_buffer) > (fs * window_time)
        proc_buffer(1) = [];
    end
    
    % Algoritmo MMPD para Picos y Valles
    if length(proc_buffer) >= 5
        v0 = proc_buffer(end);   % Muestra actual
        v1 = proc_buffer(end-1); % Muestra -1
        v2 = proc_buffer(end-2); % Muestra -2
        
        if v0 > v1
            num_upsteps = num_upsteps + 1;
        end
        
        % Detector de Pico (Período refractario de 0.45 s)
        if (v1 > v2) && (v0 <= v1) && (num_upsteps >= threshold)
            if (t_sample - last_peak_time) > 0.45
                time_peak = t_sample - dt;
                val_peak = v1;
                
                peaks_time(end+1) = time_peak;
                peaks_val(end+1)  = val_peak;
                
                if last_peak_time > 0
                    ibi_current = time_peak - last_peak_time;
                end
                last_peak_time = time_peak;
                last_peak_val  = val_peak;
                
                ppga_current = abs(last_peak_val - last_valley_val);
                num_upsteps = 0;
            end
        end
        
        % Detector de Valle
        if (v1 < v2) && (v0 >= v1)
            if (t_sample - last_peak_time) > 0.15
                time_valley = t_sample - dt;
                val_valley = v1;
                
                valleys_time(end+1) = time_valley;
                valleys_val(end+1)  = val_valley;
                
                last_valley_val = val_valley;
            end
        end
    end
    
    % CÁLCULO DEL ÍNDICE PLETISEMOGRÁFICO (SPI)
    hbi_norm  = min(max((ibi_current - 0.4) / (1.2 - 0.4), 0), 1) * 100;
    ppga_norm = min(max(ppga_current / 25000, 0), 1) * 100;
    
    spi_val = 100 - (0.7 * ppga_norm + 0.3 * hbi_norm);
    % spi_val = min(max(spi_val, 0), 100);
    
    time_history(end+1) = t_sample;
    spi_history(end+1)  = spi_val;
end

% --- GRÁFICA POST-CAPTURA DE LA SEÑAL CON PICOS Y VALLES ---
fig_post = figure('Name', 'PPG Completo - Detección de Picos y Valles', 'NumberTitle', 'off', 'Color', 'w');
ax_post = axes(fig_post);
hold(ax_post, 'on'); grid(ax_post, 'on');

plot(ax_post, time_log, clean_ppg_log, 'm-', 'LineWidth', 1.2, 'DisplayName', 'PPG');
plot(ax_post, peaks_time, peaks_val, 'ko', 'MarkerSize', 6, 'MarkerFaceColor', 'k', 'LineStyle', 'none', 'DisplayName', 'Picos');
plot(ax_post, valleys_time, valleys_val, 'co', 'MarkerSize', 6, 'MarkerFaceColor', 'c', 'LineStyle', 'none', 'DisplayName', 'Valles');

title(ax_post, 'Señal PPG Completa con Picos y Valles Detectados');
xlabel(ax_post, 'Tiempo (s)'); ylabel(ax_post, 'Amplitud');
legend(ax_post, 'Location', 'northeast');
set(ax_post, 'FontSize', 10, 'FontName', 'Times New Roman');

max_t = max(120, max(time_log));
xlim(ax_post, [0, max_t]);
```

Esta sección del script procesa la señal PPG previamente almacenada con el fin de identificar eventos cardíacos ciclo a ciclo y evaluar la respuesta del sistema nervioso autónomo. Para ello se emplea el algoritmo MMPD de detección de picos y valles, que analiza la señal muestra a muestra mediante una ventana móvil de tres puntos consecutivos (𝑣0,𝑣1,𝑣2) para reconocer cambios de pendiente. Los picos sistólicos se determinan cuando la señal pasa de ascendente a descendente, exigiendo al menos dos pasos de subida  (num_upsteps≥2)y un intervalo mínimo de 0.45s entre latidos, lo que evita confundir la onda dicrótica con un falso pico. Los valles diastólicos, por su parte, se identifican en los puntos más bajos de la señal, siempre con una separación de seguridad mayor a 0.15s respecto al pico anterior.

A partir de esta detección se calculan variables fisiológicas relevantes: el intervalo inter‑batido (IBI), que mide el tiempo exacto entre picos consecutivos (Δ𝑡); la amplitud pletismográfica (PPGA), definida como la diferencia de voltaje entre el pico y el valle ∣𝑉𝑝𝑖𝑐𝑜−𝑉𝑣𝑎𝑙𝑙𝑒∣; y el índice pletismográfico (SPI), que normaliza tanto el IBI como la PPGA en una escala de 0 a 100 %, aplicando la fórmula ponderada:

𝑆𝑃𝐼=100−(0.7×𝑃𝑃𝐺𝐴𝑛𝑜𝑟𝑚+0.3×𝐻𝐵𝐼𝑛𝑜𝑟𝑚)

Un aumento en el valor de SPI refleja una mayor actividad simpática, asociada a vasoconstricción o estados de estrés. Finalmente, el script genera una figura de validación que muestra la señal PPG continua y limpia, sobre la cual se superponen marcadores gráficos (círculos negros para picos y cian para valles), permitiendo verificar visualmente la correcta detección de los eventos cardíacos.

```matlab
if ~isempty(spi_history)
    fig2 = figure('Name', 'Evolución del SPI', 'NumberTitle', 'off', 'Color', 'w');
    ax2 = axes(fig2);
    
    spi_clean  = medfilt1(spi_history, 15);
    spi_smooth = movmean(spi_clean, 50);
    
    plot(ax2, time_history, spi_smooth, 'b-', 'LineWidth', 2, 'DisplayName', 'Índice SPI');
    hold(ax2, 'on'); grid(ax2, 'on');
    
    patch(ax2, [40 80 80 40], [0 0 100 100], [1 0.8 0.8], 'FaceAlpha', 0.4, ...
          'EdgeColor', 'none', 'DisplayName', 'Maniobra CPT (40s - 80s)');
      
    xline(ax2, 40, '--r', 'LineWidth', 1.5, 'HandleVisibility', 'off');
    xline(ax2, 80, '--r', 'LineWidth', 1.5, 'HandleVisibility', 'off');
    
    ylim(ax2, [0, 100]); 
    max_t_spi = max(120, max(time_history));
    xlim(ax2, [0, max_t_spi]);
    
    title(ax2, 'Evolución del Índice Pletismográfico (SPI) en Función del Tiempo');
    xlabel(ax2, 'Tiempo (s)'); 
    ylabel(ax2, 'Índice SPI (0 - 100)');
    legend(ax2, 'Location', 'northeast');
    set(ax2, 'FontSize', 10, 'FontName', 'Times New Roman');
    
    % Promedios impresos
    idx_basal = time_history <= 40;
    idx_cpt   = time_history > 40 & time_history <= 80;
    idx_rec   = time_history > 80;
    
    fprintf('\n=========================================\n');
    fprintf('    VALORES PROMEDIO DEL ÍNDICE SPI      \n');
    fprintf('=========================================\n');
    fprintf('1. Fase Basal (0 - 40 s):           %.2f\n', mean(spi_history(idx_basal)));
    fprintf('2. Durante CPT (40 - 80 s):         %.2f\n', mean(spi_history(idx_cpt)));
    fprintf('3. Recuperación (80 - 120 s):       %.2f\n', mean(spi_history(idx_rec)));
    fprintf('=========================================\n');
end
```

En esta sección del script se realiza la segmentación lógica del registro temporal, creando tres máscaras booleanas que dividen la señal en las etapas del experimento: fase basal (reposo, 0−40 s), fase CPT(estímulo térmico/estrés, 40−80 s) y fase de recuperación (post‑estímulo, 80−120 s). Sobre cada intervalo se aplica la función mean() a la variable spi_history, obteniendo los valores promedio del índice pletismográfico y generando una tabla comparativa en la ventana de comandos de MATLAB.

Para mejorar la calidad de la señal, se emplea un filtrado combinado: un filtro de mediana para eliminar ruido aislado y un filtro de media móvil para suavizar la curva. Finalmente, se construye la gráfica de la evolución del SPI como indicador de estrés hemodinámico, destacando visualmente la ventana temporal correspondiente al Cold Pressor Test (40−80s) y mostrando los promedios cuantitativos de las tres fases (basal, CPT y recuperación).

En donde se obtuvieron 2 resultados uno de una integrante del grupo y otros obtenidos con el docente.

##
1. Integrante del grupo
   
Señal PPG:

<img width="1600" height="680" alt="image" src="https://github.com/user-attachments/assets/6ff531db-6aec-4bbb-af08-77bfdde28117" />

 Gráfico SPI

<img width="1600" height="748" alt="image" src="https://github.com/user-attachments/assets/3972e110-2989-41af-b42e-347407b438b0" />

Índice SPI

<img width="441" height="201" alt="image" src="https://github.com/user-attachments/assets/c2710e83-642e-459c-867d-80c856ca278f" />

En el analisis de estas primeras gráficas  la respuesta al estímulo fisiológico (CPT) muestra un incremento significativo del tono simpático: el valor medio del SPI en la fase basal es de 66.05 y aumenta hasta 83.21 durante la maniobra de agua helada (40−80 s), confirmando la activación simpática asociada a vasoconstricción periférica y aceleración del ritmo cardíaco. En la fase de recuperación (80−120 s), el promedio se eleva ligeramente a 85.79, lo que evidencia la ausencia de retorno inmediato a los niveles basales y la persistencia de un efecto residual del estrés.

Para el comportamiento temporal del índice SPI, se observa que en la fase de inicialización (0−15 s) el valor cae bruscamente hasta ~15 puntos, debido al tiempo de estabilización de los filtros digitales y del búfer. Durante el CPT, entre los segundos 40 y 60, se registra una bajada transitoria (~62 puntos), seguida de un ascenso drástico hasta un pico cercano a 97 alrededor del segundo 67, lo que refleja una reacción retardada o una variación hemodinámica progresiva mientras la mano permaneció en el agua fría.

Respecto a la calidad de la señal PPG y la detección de artefactos, se identifican eventos notables: un pico de amplitud superior a 3300 unidades en 𝑡 ≈ 50 s  y otro mayor a 2500 unidades en  𝑡 ≈ 81s.  Estos artefactos coinciden con los momentos de inmersión y retiro de la mano, generando fluctuaciones bruscas de voltaje por movimiento. A pesar de estas variaciones, el algoritmo MMPD mostró robustez, logrando identificar correctamente los picos sistólicos (que se evidencian como los puntos negros) y valles diastólicos (siendo los puntos cian), lo que permitió calcular de manera continua tanto el intervalo inter‑latido (IBI) como la amplitud de pulso (PPGA).
##
2. Datos obtenidos con el Docente

Señal PPG 

   <img width="1600" height="731" alt="image" src="https://github.com/user-attachments/assets/b4a00cab-1cbf-4ffa-a2dc-87df15863828" />

Gráfico SPI

<img width="1600" height="740" alt="image" src="https://github.com/user-attachments/assets/72e5150c-c889-48f6-8b6b-27e76e847dde" />

Índice SPI

<img width="622" height="267" alt="image" src="https://github.com/user-attachments/assets/ca883861-3ec4-46a0-9497-08e670619235" />

Para esta segunda grafica el análisis de este conjunto de datos evidencia una marcada respuesta nociceptiva fisiológica durante el Cold Pressor Test (CPT). En la fase basal, el valor promedio del SPI se sitúa en 68.32, mientras que durante el estímulo de agua helada (40−80 s) asciende drásticamente a 86.97, lo que representa un incremento de ~18.65 puntos. Este aumento refleja una respuesta neurovegetativa simpática evidente, manifestada por vasoconstricción periférica (reducción en la amplitud de la onda PPG) y aumento de la frecuencia cardíaca (acortamiento de los intervalos entre picos).

En la fase de recuperación (80−120 s), el SPI se mantiene elevado en 85.88, prácticamente al mismo nivel que durante el estímulo. Esto indica que un periodo de 40 segundos posestímulo no fue suficiente para que el sujeto retornara a sus valores basales, manteniéndose activa la vasoconstricción periférica y la respuesta simpática debido al dolor residual tras retirar la mano del agua helada.

Finalmente, la síntesis comparativa con las gráficas de SPI confirma lo observado en la tabla de promedios: aunque la señal presenta oscilaciones continuas producto del ritmo respiratorio y modulaciones vasculares, el nivel medio global se desplaza de manera sostenida hacia valores altos (>85) desde el inicio del CPT hasta el final de la prueba, consolidando la evidencia de una respuesta simpática persistente.

## CONCLUSION

En el análisis de las gráficas obtenidas se confirma que el Cold Pressor Test (CPT) genera una respuesta simpática marcada, evidenciada en el incremento del SPI desde valores basales de 66.05 y 68.32 hasta promedios superiores a 83 y 86 durante el estímulo de agua helada, lo que refleja vasoconstricción periférica y aceleración del ritmo cardíaco. En la fase de recuperación, el índice se mantiene elevado (85.79 y 85.88), demostrando que un periodo de 40 segundos no es suficiente para retornar a los niveles fisiológicos iniciales y que persiste un efecto residual del dolor. El comportamiento temporal del SPI muestra una caída inicial atribuida a la estabilización de filtros y búferes, seguida de un ascenso progresivo con picos cercanos a 97, lo que evidencia una dinámica retardada de la respuesta hemodinámica. Además, se identificaron artefactos de movimiento en momentos de inmersión y retiro de la mano, con amplitudes superiores a 2500 y 3300 unidades, que generaron fluctuaciones bruscas en la señal PPG. A pesar de estas variaciones, el algoritmo MMPD demostró robustez al detectar de manera confiable picos sistólicos y valles diastólicos, permitiendo calcular de forma continua el intervalo inter‑latido (IBI) y la amplitud pletismográfica (PPGA). En síntesis, los resultados confirman que el SPI es un indicador sensible y consistente de la actividad simpática, capaz de reflejar tanto la magnitud del estímulo nociceptivo como la persistencia de sus efectos en el tiempo.

## REFERENCIAS
[1] J. M. Carrillo, S. Collado Vázquez y N. Rojo, “El Cold Pressor Test en la investigación del dolor experimental y clínico,” Biociencias, vol. 3, Universidad Alfonso X el Sabio, Madrid, 2005.

[2] M. Huiku et al., “Assessment of surgical stress during general anaesthesia,” British Journal of Anaesthesia, vol. 98, no. 4, pp. 447–455, 2007, doi: 10.1093/bja/aem004.

[3] E. J. Argüello-Prada, “The mountaineer’s method for peak detection in photoplethysmographic signals,” Revista Facultad de Ingeniería, Universidad de Antioquia, no. 90, pp. 42–50, Jan.–Mar. 2019, doi:10.17533/udea.redin.n90a06.
