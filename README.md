# README_helpdesk

# Helpdesk Tech Analytics Dashboard

Este repositorio contiene el conjunto de datos de ticketing y la configuración de diseño visual para el cuadro de mando de analítica de un servicio de soporte técnico corporativo (Helpdesk) 24/7 de una empresa tecnológica.

## 📊 1. Conjunto de Datos (`tickets_helpdesk.csv`)

El archivo principal contiene **1,000 registros** de incidencias distribuidas cronológicamente entre **julio y diciembre de 2026**. El archivo utiliza comas (`,`) como separador estándar.

### Estructura de Columnas

- **`ID_Ticket`**: Identificador único de la incidencia (desde `T001` hasta `T1000`).
- **`Fecha_Apertura`**: Fecha y hora en la que el usuario registró la incidencia.
- **`Fecha_Cierre`**: Fecha y hora de resolución. Si el ticket no está cerrado, el campo permanece vacío.
- **`Categoría`**: Clasificación técnica de la incidencia (*Acceso/Contraseñas*, *Hardware/Impresoras*, *Software Corporativo*, *Correo Electrónico*, *Telefonía*, *Infraestructura/Cloud*, *Despliegues/Git*).
- **`Prioridad`**: Nivel de criticidad (*Baja*, *Media*, *Alta*).
- **`Estado`**: Estado operativo del ticket (*Abierto*, *En Progreso*, *Cerrado*).
- **`ID_Agente`**: Código del agente asignado (`A01` a `A10`).
- **`Satisfaccion`**: Valoración del usuario (1 a 5). Se mantiene **vacía de origen** para los tickets no cerrados para preservar la integridad de las métricas de calidad (CSAT).

### Organización del Equipo y Turnos (SLA)

La asignación de agentes está gobernada por una matriz de turnos estricta:

1. **Turno de Mañana (07:00h a 15:00h, L-V):** Atendido equitativamente por los agentes **A01, A02, A03 y A04**. Gestión de incidencias operativas comunes.
2. **Turno de Tarde (15:00h a 22:00h, L-V):** Atendido equitativamente por los agentes **A05, A06, A07 y A08**. Gestión de incidencias operativas comunes.
3. **Soporte Externo (22:00h a 07:00h, Fines de Semana y Festivos):** Cubierto en exclusividad por los agentes **A09 y A10**. Presenta un volumen bajo de actividad (aprox. 6% del total) correspondiente estrictamente a **Urgencias Reales Justificadas**, catalogadas de forma obligatoria con **Prioridad Alta**.

---

## 🎨 2. Interfaz Visual (`theme_dark_premium.json`)

Para la visualización de los datos en Power BI, se incluye una plantilla de tema premium optimizada para entornos **100% oscuros**, asegurando alto contraste, jerarquía visual y un aspecto “flotante” para los objetos visuales.

### Paleta de Colores Corporativa

- **Fondo del Lienzo:** Azul oscuro con toque violeta (`#1E2245`).
- **Fondo de Gráficos e Interfaz:** Azul Marino Profundo (`#171A34`).
- **Bordes y Acentos Principales:** Morado Medio (`#9370DB`).
- **Textos de Lectura Estándar:** Beige Crudo (`#F5F5DC`), Azul Hielo (`#A5F2F3`) y Plata (`#A6A6A6`).
- **KPIs y Destacados:** Azul Hielo (`#A5F2F3`) con tipografía en contraste oscuro.
- **Resultados y Totales (Matrices):** Verde Claro Lima (`#9EFD38`).
- **Toques de Alerta:** Granate Violeta (`#6B1D4F`).

### Estilos Automatizados en Power BI

- **Tipografía:** Títulos estandarizados a tamaño **16 con fuente Segoe UI Semibold** en color azul hielo.
- **Contenedores:** Esquinas redondeadas automáticas con un radio de **12 píxeles**, bordes morados y sombreado exterior desvanecido (*dropShadow*) para simular capas flotantes.
- **Segmentadores (Slicers):** Barras y flechas de deslizamiento configuradas en beige claro y para facilitar la interactividad el cuadro de selección en verde lima.
- **Tablas y Matrices:** Cabeceras en Azul Marino Profundo con texto beige y totales destacados en verde lima brillante.

### 🔗 3. Modelo de Datos y Relaciones (Esquema Estrella)

El informe está optimizado utilizando un enfoque de modelado dimensional eficiente:

- **Tabla de Hechos:** `tickets_helpdesk.csv` (Contiene las métricas operativas y registros diarios).
- **Tabla de Dimensiones:** `Agentes-copia`
- (Tabla maestra con identificadores y nombres del personal).
- **Regla de Filtrado:** Los segmentadores, buscadores y filtros de selección interactiva utilizan de forma estricta las columnas de la tabla maestra (`Agentes`), garantizando una propagación de filtros limpia (dirección única) hacia la tabla de hechos sin sobrecargar el rendimiento del motor DAX.

## Gestión de imágenes de agentes

Durante el desarrollo se intentó incorporar imágenes dinámicas de los agentes mediante codificación Base64 y filtrado por selección en Power BI.

### Problema encontrado

Las imágenes originales tenían un tamaño excesivo para el uso previsto dentro del dashboard.

Al convertirlas a Base64:

- Las cadenas generadas alcanzaban decenas de miles de caracteres.
- Algunas transformaciones intentadas para reducir longitud provocaban que las imágenes quedaran truncadas.
- Power BI reconocía la existencia de la imagen pero no podía reconstruirla correctamente, mostrando un icono de imagen no válida.

### Análisis realizado

Se verificó:

- Relaciones entre tablas.
- Filtros cruzados entre agentes e imágenes.
- Categorías de datos (URL de imagen).
- Medidas DAX y columnas calculadas.
- Integridad de las cadenas Base64.

El origen del problema estaba relacionado con el tamaño y procesamiento de las imágenes durante la conversión.

### Solución adoptada

Reducir previamente el tamaño de las imágenes antes de su importación en Power BI.

Para una fotografía mostrada en un espacio reducido (ficha de agente o tarjeta de información) no es necesario utilizar imágenes de gran resolución.

Se suben al GitHub para utilizar la url generada y que el PowerBI busque la imagen en vez de tenerlas que tener en el dispositivo e importarlas con Base64.

Imágenes optimizadas:

- Reducen tamaño del modelo.
- Mejoran rendimiento.
- Mejor subirlas a alguna plataforma e utilizar la URL.
- Disminuyen problemas de renderizado.

### Aprendizaje

En dashboards corporativos es preferible utilizar imágenes optimizadas para pantalla antes de cargarlas en el modelo de datos, especialmente cuando se desea mostrarlas en tarjetas o visuales pequeños.

### Siguiente paso en la tarjeta de Imagen:

Se usa una imagen que representa el equipo de trabajo Helpdesk para cuando no hay ningun agente seleccionado. 

Se juntan las imagenes de los equipos, los turnos de trabajo para cuando se selecciona un turno poder ver todo el equipo que opera en ese turno.

### Medidores:

Se usan uno para la Media de Satisfacción con el limite de 4, poder visualizar el estado general o si seleccionas un ticket concreto poder ver en el medidor de un vistazo su estado de satisfacción; y otro para el % de tickets resueltos, el numero dentro del arco es el % resuelto, el numero fuera arriba es el deseado.

### Baglock-fijo

Muestra los tickets que hay actualmente abiertos. 

### Slicers:

Id tiket, Id agente/nombre agente y Prioridad, sirven para seleccionar en cada uno lo que indica el titulo. 

### Recuento:

Muestra la cantidad de tickets: Abiertos, cerrados y en curso.

### Tabla:

Id agente, nombre agente, id tiket, prioridad, estado, tiempo total de resolución en los que ya están cerrados. 

### % Tickets Resueltos por Turno:

Mosaicos con los 3 turnos con el nombre de cada uno y el % de tickets resueltos, si seleccionas uno en la tarjeta de la imagen sale el equipo de agentes encargados en ese turno y abajo el nombre del turno. 

### Borrar todos los filtros:

Un cuadro de texto que funciona como un boton que cuando con Ctrl+boton izq raton pulsas se restablecen todos los filtros y queda limpio para ver de nuevo el Dashboard en el estado general sin nada marcado, util para cuando no sabes bien que filtro hay, nota: solo funciona con Slicers, segmentadores, no funciona con tablas y graficos (como el de la fecha)

Debajo un recuadro sin ningun objeto para pulsar encima y poder salir de otro objeto que tengas seleccionados.

### Triple Tarjeta:

Tres apartados % tickets resueltos, minutos en resolver y un semaforo de Estado SLA, por defecto el semaforo muestra el color naranja ya que en general si no le decimos que muestre este color mostraria el rojo o el verde, entonces he optado por el naranja para hacer un resultado medio, ya que sumaria los que no se han cerrado aun, los abierto y en curso, no reflejando la realidad. Cuando se selecciona en cualquier otro objeto ya sea el idtiket o en la tabla o grafico de fecha se puede ya ver un dato real concreto. 

### Grafico de fechas:

Grafico con las fechas por días, mes y año, el cual se puede hacer scroll horizontal, refleja en varios colores los tickets cerrados, abiertos y en curso. Al pulsar una barra todo el dashboard se mueve mostrando el dato en los diferentes objetos. 

## Transformaciones realizadas en Power Query

Para la construcción del modelo se realizaron varias transformaciones previas antes de cargar los datos:

- Conversión de campos de fecha y hora a tipos compatibles con Power BI para permitir segmentación temporal y cálculos.
- Creación de una tabla de objetivos SLA independiente para desacoplar la lógica de negocio de la tabla de tickets.
- Conversión de tiempos SLA desde horas a minutos para trabajar con una única unidad de medida en todos los cálculos.
- Uso de índices auxiliares para pruebas de comportamiento en KPI y validación de objetivos.

---

## Medidas DAX implementadas

Durante el desarrollo se crearon medidas para evitar cálculos estáticos y permitir respuestas dinámicas a filtros y segmentadores.

Entre ellas:

### Promedio de tiempo de resolución

Calcula el tiempo medio de resolución únicamente para los tickets cerrados, evitando distorsionar los resultados con incidencias todavía abiertas o en curso.

### Porcentaje de tickets resueltos

Porcentaje dinámico que permite evaluar el volumen de incidencias resueltas respecto al total existente bajo cualquier filtro aplicado.

### Satisfacción media

Promedio de valoración de los usuarios únicamente sobre tickets finalizados.

### Estado SLA

Indicador visual tipo semáforo que evalúa el estado general del servicio en función de los tiempos medios de resolución respecto al objetivo definido.

---

## Problemas encontrados durante el modelado

### KPI y comparación de objetivos

Uno de los principales retos fue comprender el comportamiento interno del visual KPI de Power BI.

Se detectó que:

- Las columnas son agregadas automáticamente.
- Una columna utilizada como objetivo podía generar recuentos no deseados.
- El KPI está especialmente orientado a métricas donde un valor superior es mejor.

Esto provocó resultados visuales poco intuitivos al trabajar con tiempos de resolución, donde un valor más bajo representa una mejora.

Como alternativa se optó por complementar el dato mediante indicadores visuales adicionales.

---

### Unificación de unidades temporales

En una primera fase coexistían valores expresados en:

- Horas.
- Minutos.
- Porcentajes.

La comparación entre unidades diferentes producía resultados difíciles de interpretar.

Se decidió utilizar los minutos como unidad principal para todos los cálculos relacionados con resolución y SLA.

---

### Relaciones de filtrado

Se realizaron diversas pruebas para verificar que:

- Los filtros de agente afectaban correctamente a tickets.
- Las imágenes respondían a los mismos filtros que el resto de elementos.
- Los visuales mostraban resultados consistentes bajo selecciones múltiples.

---

## Decisiones de diseño adoptadas

Durante el proyecto se priorizó:

- Lectura rápida.
- Alto contraste visual.
- Reducción de información redundante.
- Máximo aprovechamiento del espacio disponible.

Se descartaron varios diseños intermedios por generar exceso de elementos repetidos o por dificultar la interpretación inmediata de los indicadores principales.

---

## Lecciones aprendidas

Este proyecto ha servido para consolidar conocimientos sobre:

- Modelado dimensional.
- Relaciones uno a muchos.
- Segmentadores y contexto de filtro.
- Power Query.
- Medidas DAX.
- Gestión de imágenes dinámicas.
- Diseño de dashboards operativos.

También permitió comprobar que una parte importante del trabajo en Power BI no consiste únicamente en construir visuales, sino en decidir qué información aporta valor real y cuál puede eliminarse para mejorar la experiencia del usuario.

---

NOTA FINAL:

> El dashboard está diseñado para funcionar tanto como cuadro de mando general del servicio HelpDesk como herramienta de análisis detallado mediante filtrado por agente, ticket, prioridad, fecha o turno de soporte.
>