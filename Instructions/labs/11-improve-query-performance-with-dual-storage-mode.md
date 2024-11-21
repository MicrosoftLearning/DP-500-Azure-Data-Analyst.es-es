---
lab:
  title: Mejora del rendimiento de las consultas con el modo de almacenamiento dual
  module: Optimize enterprise-scale tabular models
---

# Mejora del rendimiento de las consultas con el modo de almacenamiento dual

## Información general

**El tiempo estimado para completar el laboratorio es de 30 minutos**

En este laboratorio, mejorarás el rendimiento de un modelo compuesto con el establecimiento de algunas tablas para usar el modo de almacenamiento dual.

En este laboratorio, aprenderá a:

- Establecer el modo de almacenamiento dual.

- Usa el Analizador de rendimiento para revisar las actividades de actualización.

## Introducción

En este ejercicio prepararás el entorno.

### Clonación del repositorio para este curso

1. En el menú de inicio, abre el símbolo del sistema

    ![](../images/command-prompt.png)

1. En la ventana del símbolo del sistema, ve a la unidad D escribiendo:

    `d:` 

   Presione Entrar.

    ![](../images/command-prompt-2.png)


1. En la ventana del símbolo del sistema, escribe el siguiente comando para descargar los archivos del curso y guardarlos en una carpeta denominada DP500.
    
    `git clone https://github.com/MicrosoftLearning/DP-500-Azure-Data-Analyst DP500`
   
1. Cuando se haya clonado el repositorio, cierra la ventana del símbolo del sistema. 
   
1. Abre la unidad D en el explorador de archivos para asegurarte de que se han descargado los archivos.

### Configurar Power BI Desktop

En esta tarea abrirás una solución de Power BI Desktop desarrollada previamente.

1. Para abrir Explorador de archivos, en la barra de tareas, selecciona el acceso directo **Explorador de archivos**.

2. Ve a la carpeta **D:\DP500\Allfiles\11\Starter**.

3. Para abrir un archivo de Power BI Desktop desarrollado previamente, haz doble clic en el archivo **Sales Analysis - Improve query performance with dual storage mode.pbix**).

4. Si aparece un mensaje sobre un riesgo de seguridad potencial, léelo y luego selecciona **Aceptar**.

5. Si se te pide que apruebes la ejecución de una consulta de base de datos nativa, selecciona **Ejecutar**.

6. Para guardar el archivo, en la ficha de cinta **Archivo**, selecciona **Guardar como**.

7. En la ventana **Guardar como**, ve a la carpeta **D:\DP500\Allfiles\11\MySolution**.

8. Seleccione **Guardar**.

### Revisar el informe

En esta tarea revisarás el informe desarrollado previamente.

1. En Power BI Desktop, en la esquina inferior derecha de la barra de estado, observa que el modo de almacenamiento es Mixto.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image3.png)

    *Un modelo mixto consta de tablas de diferentes grupos de origen. Este modelo tiene una tabla de importación que obtiene sus datos de un libro de Excel. Las tablas restantes usan una conexión DirectQuery a una base de datos de SQL Server, que es el almacenamiento de datos.*

2. Revisa el diseño de informe.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image4.png)

    *Esta página del informe tiene un título y dos objetos visuales. El objeto visual de segmentación de datos permite filtrar por un solo año fiscal, mientras que el objeto visual del gráfico de columnas muestra las ventas trimestrales y los importes de destino. Mejorarás el rendimiento del informe si estableces algunas tablas para usar el modo de almacenamiento dual.*

### Revisión del modelo de datos

En esta tarea revisarás el modelo de datos desarrollado previamente.

1. Cambia a la vista **Modelo**.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image5.png)

2. Usa el diagrama de modelos para revisar el diseño del modelo.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image6.png)

    *El modelo consta de tres tablas de dimensiones y dos tablas de hechos. La tabla de hechos **Ventas** representa los detalles del pedido de ventas, mientras que la tabla **Destinos** representa los objetivos de ventas trimestrales. Es un diseño de esquema de estrella clásico. La barra en la parte superior de algunas de las tablas indica que usan el modo de almacenamiento DirectQuery. Cada tabla que tiene una barra azul pertenece al mismo grupo de origen.*

    *En este laboratorio, configurarás algunas tablas para usar el modo de almacenamiento dual.*

## Configuración del modo de almacenamiento dual

En este ejercicio, configurarás el modo de almacenamiento dual.

*Una tabla de modelos que usa el modo de almacenamiento dual utiliza el modo de almacenamiento de importación y DirectQuery al mismo tiempo. Power BI determina el modo de almacenamiento más eficaz que se usará según la consulta, esforzándose por usar el modo de importación siempre que sea posible, ya que es más rápido.*

### Uso del analizador de rendimiento

En esta tarea abrirás el Analizador de rendimiento y lo usarás para inspeccionar los eventos de actualización.

1. Cambie a la vista **Informe**.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image7.png)

2. Para inspeccionar los eventos de actualización visual, en la ficha de cinta **Ver**, en el grupo de paneles **Mostrar**, selecciona **Analizador de rendimiento**.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image8.png)

3. En el panel **Analizador de rendimiento** (situado a la izquierda del panel **Visualizaciones**), selecciona **Iniciar grabación**.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image9.png)

    *El analizador de rendimiento inspecciona y muestra la duración necesaria para actualizar o renovar los objetos visuales. Cada objeto visual emite al menos una consulta a la base de datos de origen. Para obtener más información, consulta [Uso del Analizador de rendimiento para examinar el rendimiento de los elementos de los informes en Power BI Desktop](https://docs.microsoft.com/power-bi/create-reports/desktop-performance-analyzer).*

4. Selecciona **Actualizar objetos visuales**.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image10.png)

5. En el panel **Analizador de rendimiento**, expande el objeto visual **Segmentación de datos** y observa el evento de consulta directa.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image11.png)

    *Cada vez que veas un evento de consulta directa, indica que Power BI usó el modo de almacenamiento DirectQuery para recuperar los datos de la base de datos de origen.*

6. Expande el objeto visual **Sales Result by Fiscal Quarter** (Resultado de ventas por trimestre fiscal) y observa que también registró un evento de consulta directa.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image12.png)

    *Siempre configurarás un objeto visual de segmentación mediante uno o varios campos de la misma tabla. No es posible usar campos de tablas diferentes para configurar una segmentación de datos. Además, una segmentación de datos casi siempre usa campos de una tabla de dimensiones. Por lo tanto, para mejorar el rendimiento de las consultas de los objetos visuales de segmentación, asegúrate de almacenar datos importados. En este caso, puesto que las tablas de dimensiones usan el modo de almacenamiento DirectQuery, puedes establecerlas en modo de almacenamiento dual. Dado que las tablas de dimensiones almacenan pocas filas (relativas a las tablas de hechos), no debería dar lugar a una caché de modelos excesivamente grande.*

### Configuración del modo de almacenamiento dual

En esta tarea, establecerás todas las tablas de dimensiones para usar el modo de almacenamiento dual.

1. Cambia a la vista **Modelo**.

2. Selecciona el encabezado de la tabla **Product**.

3. Al presionar la tecla **Ctrl**, selecciona también los encabezados de las tablas **Fecha de pedido** y **Territorio de ventas**.

4. En el panel **Propiedades**, expande la sección **Avanzadas**.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image13.png)

5. En la lista desplegable **Modo de almacenamiento**, selecciona **Dual**.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image14.png)

6. Cuando se te pida que confirmes la actualización, selecciona **Aceptar**.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image15.png)

    *La advertencia te informa de que Power BI Desktop puede tardar mucho tiempo en importar datos en las tablas del modelo.*

7. En el diagrama del modelo, observa la barra seccionada en la parte superior de cada tabla de dimensiones.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image16.png)

    *Una barra seccionada indica el modo de almacenamiento dual.*

### Revisar el informe

En esta tarea revisarás el informe desarrollado previamente.

1. Cambie a la vista **Informe**.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image17.png)

2. En el panel **Analizador de rendimiento**, selecciona **Borrar**.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image18.png)

3. Actualiza los objetos visuales.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image19.png)

4. Observa que el objeto visual de segmentación ya no usa una conexión de consulta directa.

    *Power BI consulta la memoria caché del modelo de datos importados, por lo que la segmentación de datos ahora se actualiza más rápidamente.*

5. Pero ten en cuenta que el objeto visual del gráfico de columnas sigue usando una conexión de consulta directa.

    *Esto se debe a que el campo **Importe de ventas** es una columna de la tabla **Ventas**, que usa el modo de almacén DirectQuery.*

6. Selecciona los objetos visuales de gráfico de columnas y luego en el panel **Visualizaciones**, desde el área **Valores**, quita el campo **Importe de ventas**.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image20.png)

7. Quita también los dos campos del área **Información sobre herramientas**.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image21.png)

    *Ambas medidas dependen de la columna **Importe de ventas**.*

8. En el panel **Analizador de rendimiento**, expande para abrir el último evento de actualización y observa que el objeto visual del gráfico de columnas ya no usa una conexión de consulta directa.

    *Esto se debe a que el objeto visual de gráfico de columnas ahora solo usa dos tablas, ambas se almacenan en caché en el modelo. La tabla **Fecha del pedido** usa el modo de almacenamiento dual, mientras que la tabla **Destinos** usa el modo de almacenamiento de importación.*

    *Ahora has mejorado el rendimiento de consultas específicas en las que Power BI puede recuperar datos de la caché del modelo. La conclusión clave es que las tablas de dimensiones relacionadas con las tablas de hechos de DirectQuery normalmente deben establecerse en modo de almacenamiento dual. De este modo, cuando se consulta mediante una segmentación de datos, las consultas serán rápidas.*

    *Podrías optimizar aún más el modelo para mejorar el rendimiento de las consultas incorporando agregaciones. Pero esa mejora será el objetivo de aprendizaje de un laboratorio diferente.*

### Finalización

En esta tarea finalizarás.

1. Guarde el archivo de Power BI Desktop.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image22.png)

2. Cierre Power BI Desktop.
