Informe final de conclusiones
1.	Resumen ejecutivo y contexto de la campaña
2.	Calidad del dato, preparación y rigor estadístico
3.	Eficiencia vs. volumen: lectura porcentual y absoluta de la conversión
4.	Dinámica de contacto y duración de las llamadas
5.	Otras variables relevantes: perfil demográfico, ciclo de vida del cliente y macroeconomía
6.	Limitaciones del análisis y recomendaciones accionables
7.	Contratiempos y otras circunstancias
Borrador de informe de conclusiones
1. Resumen ejecutivo y contexto de la campaña
El análisis abarca 43.000 registros resultantes de cruzar los datos de clientes (customer-details.xlsx, con registros de 2012 a 2014) y la información de campañas de marketing telefónico y contexto macroeconómico (bank-additional.csv).
La variable objetivo (y) muestra una fuerte asimetría de clases: aproximadamente el 11,27 % de los contactos concluyen en suscripción del depósito a plazo (YES), frente a un 88,73 % que no contratan (NO). En consecuencia, una campaña de captación telefónica de este tipo no se evalúa solo por el porcentaje de acierto global, sino por cómo se reparte la conversión entre distintos segmentos y qué palancas explican las diferencias de respuesta.
2. Calidad del dato, preparación y rigor estadístico
La fase de limpieza y transformación resolvió problemas críticos que habrían distorsionado el análisis si se hubieran dejado tal cual:
•	Unión e integridad: Se consolidaron ambas fuentes mediante ID / id_, eliminando columnas auxiliares redundantes (Unnamed: 0_x, Unnamed: 0_y e id_) y verificando la ausencia de filas duplicadas exactas.
•	Tipado y formatos: Se convirtieron a numérico (float64) indicadores económicos almacenados como texto con coma decimal (cons.price.idx, cons.conf.idx, euribor3m, nr.employed) y se normalizaron las fechas (date desde formato textual en español y Dt_Customer a datetime64), además de estandarizar cadenas de texto y documentar valores nulos (SIN_DATO en categorías seleccionadas y mediana en variables numéricas continuas).
•	Variables derivadas: Se incorporaron y_num para facilitar el cálculo de tasas de conversión, contactado_prev_pdays para separar el código centinela 999 (sin contacto previo registrado en campaña) del resto de registros, y variables temporales (anio_contacto, mes_contacto).
•	Uso de medianas y métricas de forma: Variables como duration, campaign y previous presentan marcada asimetría positiva y curtosis elevada, lo que indica colas largas y valores extremos. Por ello, acompañar la media con la mediana (y revisar gráficos como boxplots acotados) resulta indispensable para describir el comportamiento típico sin que los extremos sesguen la lectura.
3. Eficiencia vs. volumen: lectura porcentual y absoluta de la conversión
Uno de los hallazgos principales del análisis es la diferencia entre tasa de conversión (%) y volumen absoluto de contrataciones (YES):
•	En términos porcentuales (eficiencia):
o	El historial de campañas anteriores (poutcome) marca una brecha muy clara: quienes tuvieron éxito previo (SUCCESS) alcanzan una tasa de conversión en torno al 65,32 %, frente al 14,23 % en FAILURE y al 8,82 % en NONEXISTENT. De forma coherente, contactado_prev_pdays == 1 presenta una conversión cercana al 64,04 % frente al 9,24 % de quienes no registran contacto previo (999).
o	Por ocupación (job), destacan en tasa porcentual los segmentos STUDENT (31,34 %) y RETIRED (25,20 %), seguidos por UNEMPLOYED (14,39 %). Estos grupos suelen tener dinámicas financieras o de disponibilidad distintas que favorecen una mayor propensión relativa al producto.
•	En términos absolutos (masa crítica):
o	Aunque STUDENT y RETIRED sobresalen en porcentaje, su peso numérico dentro del universo total es menor que el de ocupaciones amplias como ADMIN., BLUE-COLLAR o TECHNICIAN, y la gran mayoría de clientes parten de NONEXISTENT (pdays == 999).
o	Por tanto, el volumen total de suscripciones (YES) depende en gran medida de estos grupos mayoritarios: mejorar ligeramente la conversión en ADMIN., TECHNICIAN o BLUE-COLLAR—especialmente cuando existe un antecedente favorable o en bases amplias sin contacto previo— puede generar un impacto absoluto notable, siempre vigilando el tamaño muestral al realizar cruces múltiples.
4. Dinámica de contacto y duración de las llamadas
•	Duración (duration): Las operaciones con resultado YES presentan duraciones sustancialmente mayores que las NO (con una media cercana a 551,62 s frente a 220,43 s, y medianas de 449,0 s frente a 163,0 s). Sin embargo, desde una perspectiva analítica rigurosa, duration es una variable ex post (se conoce al finalizar la llamada): una llamada corta suele reflejar un rechazo temprano, mientras que una llamada larga acompaña a conversaciones donde hay interés y explicación de condiciones. El objetivo operativo no es forzar llamadas largas sin criterio, sino sostener conversaciones de calidad cuando el cliente muestra receptividad.
•	Frecuencia de contactos en la campaña (campaign): La tasa de conversión tiende a descender a medida que aumentan los intentos dentro de la misma campaña (pasando de 13,02 % en el primer contacto a 5,53 % cuando se alcanzan 6 o más contactos). Insistir repetidamente sobre un mismo cliente muestra rendimientos decrecientes y mayor riesgo de saturación.
•	Canal de contacto (contact): El contacto vía telefonía móvil (CELLULAR) muestra una tasa de conversión superior (14,74 %) frente a telefonía fija (TELEPHONE, 5,16 %), sugiriendo diferencias de accesibilidad o efectividad del canal.
5. Otras variables relevantes: perfil demográfico, ciclo de vida del cliente y macroeconomía
Más allá de los factores comentados en primer lugar, el dataset incorpora otras dimensiones que conviene integrar en la lectura global:
•	Entorno macroeconómico (euribor3m, emp.var.rate, cons.price.idx, cons.conf.idx, nr.employed):
o	Los datos muestran que las suscripciones (YES) se asocian con niveles de euribor3m más bajos en promedio y, sobre todo, en mediana (1,40 en YES frente a 4,86 en NO), así como correlaciones lineales negativas moderadas de y_num con variables como nr.employed y emp.var.rate.
o	En términos económicos, el contexto de tipos de interés condicionan el atractivo relativo de un depósito a plazo frente a otras alternativas o frente a las necesidades de liquidez de los hogares.
•	Variables demográficas y del hogar (age, marital, education, Income, Kidhome, Teenhome):
o	age presenta una ligera asimetría positiva y mayor presencia de perfiles retirados o jóvenes en los extremos con tasas altas, mientras que Income muestra una distribución amplia y bastante simétrica en su rango general, con diferencias moderadas entre YES y NO.
o	Kidhome y Teenhome reflejan la estructura familiar del hogar, un factor que puede influir en la capacidad de inmovilizar ahorro a plazo o en la disponibilidad para atender llamadas.
•	Relación previa y actividad digital (Dt_Customer, anio_registro_hoja, NumWebVisitsMonth, default, housing, loan):
o	Dt_Customer y anio_registro_hoja permiten situar la antigüedad del alta del cliente en el banco (2012–2014) frente a la fecha de contacto de la campaña (date).
o	NumWebVisitsMonth aporta una señal de interacción digital mensual del cliente, mientras que default, housing y loan describen su situación crediticia/patrimonial básica (con un volumen relevante de registros sin dato en default, lo que aconseja tratar esa categoría con cautela).
6. Limitaciones del análisis y recomendaciones accionables
•	Limitaciones: El análisis es de carácter descriptivo y exploratorio (no establece causalidad pura ni incluye modelado predictivo multivariante); además, variables como duration no están disponibles antes de realizar la llamada y varias columnas presentan valores ausentes o categorías SIN_DATO / NONEXISTENT que deben considerarse al interpretar resultados.
•	Recomendaciones:
1.	Priorizar leads con historial favorable (poutcome == 'SUCCESS' / contactado_prev_pdays == 1) por su altísima eficiencia relativa.
2.	Combinar foco de eficiencia y escala: Mantener campañas específicas para segmentos con alta tasa porcentual (STUDENT, RETIRED) y, al mismo tiempo, optimizar guiones y segmentación en los grandes grupos de volumen (ADMIN., TECHNICIAN, BLUE-COLLAR y NONEXISTENT) para maximizar el número total de altas.
3.	Limitar el número de reintentos (campaign) y priorizar CELLULAR, evitando insistir en exceso cuando no hay respuesta favorable.
4.	Contextualizar las metas comerciales según el entorno macroeconómico (euribor3m y variables de empleo), ajustando expectativas y argumentario cuando cambian las condiciones de tipos y ahorro.

7. Contratiempos y otras circunstancias
•	Al cargar el Excel “customer-details.xlsx” nos encontramos con la circunstancia de que tiene varias paginas. Para no caer en la trampa de unificarlo en Excel recurrimos a la IA para dar una solución.
•	Tuve que recurrir a IA intensamente en primer lugar para intentar conocer los significados de las columnas y sus implicaciones.
•	Posteriormente las relaciones entre estas columnas eran ligeramente más evidentes pero sus implicaciones en como se afectan entre ellas no tanto.
•	Aunque los pasos de limpieza y transformación estaban claros el uso de IA ha sido fácil para expresar el código de una manera limpia y estructurada.
•	No obstante en la fase de Analisis Descriptivo el criterio sobre las preguntas de negocio y visualización tanto en términos porcentuales como absolutos ha prevalecido.


