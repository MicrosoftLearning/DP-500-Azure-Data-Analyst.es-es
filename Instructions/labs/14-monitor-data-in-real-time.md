---
lab:
  title: Supervisión de datos en tiempo real
  module: Implement advanced data visualization techniques by using Power BI
---

# Supervisión de datos en tiempo real

## Información general

**El tiempo estimado para completar el laboratorio es de 30 minutos**

En este laboratorio configurarás un informe para que use la actualización automática de páginas. De este modo, será posible que los consumidores de informes supervisen los resultados de las ventas por Internet en tiempo real.

En este laboratorio, aprenderá a:

- Usar el analizador de rendimiento para revisar las actividades de actualización.

- Configurar la actualización automática de páginas.

- Crea y usa la característica de detección de cambios.

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

### Configuración de la base de datos

En esta tarea usarás SQL Server Management Studio (SSMS) para configurar la base de datos mediante la ejecución de dos scripts.

1. Para abrir SSMS, en la barra de tareas, selecciona el acceso directo de **SSMS**.

    ![](../images/dp500-monitor-data-in-real-time-image1.png)

2. En la ventana **Conectar a servidor**, asegúrate de que la lista desplegable **Nombre del servidor** esté establecida en **localhost** y que la lista desplegable Autenticación esté establecida en **Autenticación de Windows**.
    ![](../images/dp500-monitor-data-in-real-time-image2.png)

3. Seleccione **Conectar**.

4. Para abrir un archivo de script, en el menú **Archivo**, selecciona **Abrir** > **Archivo**.

5. En la ventana **Abrir archivo**, ve a la carpeta **D:\DP500\Allfiles\14\Assets**.

6. Selecciona el archivo **1-Setup.sql**.

    ![](../images/dp500-monitor-data-in-real-time-image4.png)

7. Seleccione **Open** (Abrir).

    ![](../images/dp500-monitor-data-in-real-time-image5.png)

8. Revisa el script.

    *Este script crea una tabla denominada **FactInternetSalesRealTime**. Un script diferente cargará datos en esta tabla para simular una carga de trabajo en tiempo real de pedidos de ventas por Internet.*

9. Para ejecutar un script, en la barra de herramientas, selecciona **Ejecutar** (o presiona **F5**).

    ![](../images/dp500-monitor-data-in-real-time-image6.png)

10. Para cerrar el archivo, en el menú **Archivo**, selecciona **Cerrar**.

11. Abre el archivo **2-InsertOrders.sql**.

    ![](../images/dp500-monitor-data-in-real-time-image7.png)

12. Revisa también este script.

    *Este script ejecuta un bucle infinito. Para cada bucle, inserta un pedido de venta y luego retrasa un período aleatorio de 1 a 15 segundos.*

13. Ejecuta el script y déjalo en ejecución hasta el final del laboratorio.

### Configurar Power BI Desktop

En esta tarea abrirás una solución de Power BI Desktop desarrollada previamente.

1. Para abrir Explorador de archivos, en la barra de tareas, selecciona el acceso directo **Explorador de archivos**.

2. Ve a la carpeta **D:\DP500\Allfiles\14\Starter**.

3. Para abrir un archivo de Power BI Desktop desarrollado previamente, haz doble clic en el archivo **Internet Sales - Monitor data in real time.pbix**.

4. Para guardar el archivo, en la ficha de cinta **Archivo**, selecciona **Guardar como**.

5. En la ventana **Guardar como**, ve a la carpeta **D:\P500\Allfiles\14\MySolution**.

6. Seleccione **Guardar**.

### Revisar el informe

En esta tarea revisarás el informe desarrollado previamente.

1. En Power BI Desktop, revisa la página del informe.

    ![](../images/dp500-monitor-data-in-real-time-image9.png)

    *Esta página del informe tiene un título y dos objetos visuales. El objeto visual de tarjeta muestra el número de pedidos de ventas, mientras que el objeto visual del gráfico de barras muestra el importe de ventas de cada subcategoría de bicicletas.*

2. Para actualizar el informe, en la ficha de cinta **Ver**, desde el interior del grupo de paneles **Mostrar**, selecciona **Analizador de rendimiento**.

    ![](../images/dp500-monitor-data-in-real-time-image10.png)

3. En el panel **Analizador de rendimiento** (situado a la derecha del panel **Visualizaciones**), selecciona **Iniciar grabación**.

    ![](../images/dp500-monitor-data-in-real-time-image11.png)

    *El analizador de rendimiento inspecciona y muestra la duración necesaria para actualizar o renovar los objetos visuales. Cada objeto visual emite al menos una consulta a la base de datos de origen. Para obtener más información, consulta [Uso del Analizador de rendimiento para examinar el rendimiento de los elementos de los informes en Power BI Desktop](https://docs.microsoft.com/power-bi/create-reports/desktop-performance-analyzer).*

4. Selecciona **Actualizar objetos visuales**.

    ![](../images/dp500-monitor-data-in-real-time-image12.png)

5. Observa que los objetos visuales del informe se actualizan para mostrar los resultados de ventas de Internet más recientes.

    *Al desarrollar un informe que se conecta a un modelo de DirectQuery local, no es posible actualizar el informe mediante el comando **Actualizar** (ubicado en la ficha de cinta **Inicio**). Esto se debe a que Power BI Desktop actualiza las conexiones de la tabla DirectQuery en su lugar. Para actualizar los objetos visuales del informe, sigue los pasos que acabas de realizar. Cuando se publica en el servicio Power BI, los consumidores de informes podrán seleccionar **Actualizar** en la barra de acciones para actualizar los objetos visuales del informe.*

    *Al diseñar un informe para el análisis en tiempo real, debe haber una mejor manera que pedir a los usuarios que actualicen constantemente la página del informe. Lograrás esa mejor manera al configurar la actualización automática de páginas en el ejercicio siguiente.*

## Configuración de la actualización automática de páginas

En este ejercicio, configurarás la actualización automática de páginas y experimentarás mediante la característica de detección de cambios.

*La actualización automática de páginas requiere al menos una tabla de modelo establecida para usar el modo de almacenamiento DirectQuery.*

### Configuración de la actualización automática de páginas

En esta tarea, configurarás la actualización automática de páginas.

1. Para seleccionar la página del informe, selecciona primero un área vacía de la página del informe.

2. En el panel **Visualizaciones**, selecciona el icono de formato (pincel).

    ![](../images/dp500-monitor-data-in-real-time-image13.png)

3. Cambia la configuración **Actualización de página** (última de la lista) a **Activado**.

    ![](../images/dp500-monitor-data-in-real-time-image14.png)

    *La actualización automática de páginas es una configuración de nivel de página. Puedes habilitarla para páginas específicas en el informe.*

4. En el panel **Analizador de rendimiento**, observa que los objetos visuales de informe se actualizan.

5. En el panel **Visualizaciones**, expande la configuración de **Actualización de páginas**.

    ![](../images/dp500-monitor-data-in-real-time-image15.png)

6. Ten en cuenta que, de forma predeterminada, la página se actualizará cada 30 minutos.

7. Modifica la configuración para actualizar la página cada 5 segundos.

    ![](../images/dp500-monitor-data-in-real-time-image16.png)

    *Importante: este intervalo de actualización frecuente te ayudará a trabajar eficazmente a través de este laboratorio. Pero ten cuidado, ya que establecer un intervalo de actualización frecuente podría afectar significativamente al rendimiento de la base de datos de origen y a otros usuarios que ven el informe.*

    *Dado que un pedido de ventas por Internet se carga cada 1-15 segundos, a veces la actualización de la página recupera los mismos resultados (porque la base de datos no registró pedidos en los últimos cinco segundos). Preferiblemente, los objetos visuales de informe solo se actualizan cuando sea necesario. Configurarás la característica de detección de cambios para hacerlo en la siguiente tarea.*

    *Una vez publicado en el servicio Power BI, los intervalos de actualización inferiores a 30 minutos requieren que guardes el informe en un área de trabajo asignada a la capacidad Premium. Además, un administrador de capacidad debe habilitar y configurar la capacidad para permitir intervalos frecuentes. Para más información, consulta [Actualización automática de páginas en Power BI](https://docs.microsoft.com/power-bi/create-reports/desktop-automatic-page-refresh).*

### Configuración de la detección de cambios

En esta tarea configurarás la detección de cambios.

1. En la configuración de **Actualización de página**, establece la lista desplegable **Tipo de actualización** en **Detección de cambios**.

    ![](../images/dp500-monitor-data-in-real-time-image17.png)

2. Para crear una medida de detección de cambios, selecciona el vínculo **Agregar detección de cambios**.

    ![](../images/dp500-monitor-data-in-real-time-image18.png)

3. En la ventana **Detección de cambios**, observa que la configuración predeterminada es crear una nueva medida.

    ![](../images/dp500-monitor-data-in-real-time-image19.png)

4. En la lista desplegable **Elegir un cálculo**, selecciona **Recuento (único)**.

    ![](../images/dp500-monitor-data-in-real-time-image20.png)

5. En el panel **Campos** (situado a la derecha, dentro de la ventana), desplázate hacia abajo para buscar la tabla **Ventas por Internet**.

6. Selecciona el campo **Pedido de ventas** y observa que la ventana la agregó al cuadro **Elegir un campo para aplicarlo**.

    ![](../images/dp500-monitor-data-in-real-time-image21.png)

7. Establece la opción de configuración **Comprobar si hay cambios cada** en 5 segundos.

    ![](../images/dp500-monitor-data-in-real-time-image22.png)

8. Seleccione **Aplicar**.

    ![](../images/dp500-monitor-data-in-real-time-image23.png)

9. En el panel **Campos**, dentro de la tabla **Ventas por Internet**, observa la adición de una medida de detección de cambios.

    ![](../images/dp500-monitor-data-in-real-time-image24.png)

    *Power BI ahora usa la medida de detección de cambios para consultar la base de datos de origen cada cinco segundos. Cada vez, Power BI almacena el resultado para que pueda compararlo la próxima vez que se utilice. Cuando los resultados difieren, significa que los datos han cambiado (en este caso, la base de datos insertó nuevos pedidos de ventas por Internet). En este caso, Power BI actualiza todos los objetos visuales de página de informe.*

    *Una vez publicado en el servicio Power BI, Power BI solo admite medidas de detección de cambios para las capacidades Premium.*

10. En el panel **Analizador de rendimiento**, selecciona **Borrar**.

    ![](../images/dp500-monitor-data-in-real-time-image25.png)

11. Observa que el Analizador de rendimiento muestra consultas de detección de cambios.

12. Observa que a veces se producen varias consultas de detección de cambios antes de que Power BI Desktop actualice los objetos visuales de informe.

    *Esto se debe a que la base de datos no insertó ningún nuevo pedido de ventas por Internet en ese momento. Esta configuración ahora es más eficaz porque los objetos visuales de informe solo se actualizan cuando sea necesario.*

### Finalización

En esta tarea finalizarás.

1. Guarde el archivo de Power BI Desktop.

    ![](../images/dp500-monitor-data-in-real-time-image26.png)

2. Cierre Power BI Desktop.

3. En SSMS, para dejar de ejecutar el script, en la barra de herramientas, selecciona **Detener** (o presiona **Alt+Inter**).

    ![](../images/dp500-monitor-data-in-real-time-image27.png)

4. Cierra el archivo de script.

5. Abre el archivo **3-Cleanup.sql**.

    ![](../images/dp500-monitor-data-in-real-time-image28.png)

    *Este script quita la tabla **FactInternetSalesRealTime**.*

6. Ejecute el script.

7. Cierre SSMS.
