---
lab:
  title: '27: consultas Kusto de Microsoft Sentinel para orígenes de datos de Microsoft Entra'
  learning path: '04'
  module: Module 04 - Plan and Implement and Identity Governance Strategy
---

# Laboratorio 27: consultas Kusto de Microsoft Sentinel para orígenes de datos de Microsoft Entra

**Nota:** este laboratorio requiere un pase para Azure. Consulta el laboratorio 00 para obtener instrucciones.

## Escenario del laboratorio

Microsoft Sentinel es una solución SIEM y SOAR nativa de la nube de Microsoft.  Mediante la conexión de orígenes de datos desde Microsoft y soluciones de seguridad de terceros, tienes la capacidad de ejecutar tareas de operaciones de seguridad.  En este ejercicio de laboratorio, creará un área de trabajo de Microsoft Sentinel con conectores de datos vinculados a Microsoft Entra ID para ejecutar consultas de búsqueda mediante el lenguaje de consulta Kusto (KQL). 

#### Tiempo estimado: 30 minutos

### Ejercicio 1: configurar Microsoft Sentinel para consultas de Kusto

#### Tarea 1: crear un área de trabajo de Microsoft Sentinel

1. Inicia sesión en  [https://portal.azure.com](https://portal.azure.com)  como Administrador global.

1. Busca y selecciona **Microsoft Sentinel**. 

1. Selecciona **+ Crear** en la esquina superior izquierda.

1. En el icono **Agregar Microsoft Sentinel a un área de trabajo**, seleccione **+ Crear un área de trabajo nueva**.

1. En **Grupo de recursos**, selecciona **Crear nuevo** e introduce **Sentinel-RG**.

1. Asigna un nombre al área de trabajo.  Ejemplo: SentinelLogAnalytics.

1. Seleccione una Región cercana.

1. Seleccione **Revisar y crear** y, a continuación, **Crear**.

1. Una vez completada la implementación del área de trabajo de Log Analytics, elige el botón **Actualizar**. Selecciona después el área de trabajo y selecciona **Agregar**.  Esto agregará el área de trabajo a Microsoft Sentinel y abrirá Microsoft Sentinel.

1. Si se te solicita, selecciona **Aceptar** para activar la prueba gratuita de Microsoft Sentinel.

#### Tarea 2: Agregar Microsoft Entra ID como origen de datos
    **Note** - As of 2/8/2024, the data source is now Microsoft Entra ID.

1. En **Microsoft Sentinel**, navega en el menú hasta **Administración de contenido** y selecciona **Centro de contenido**.

1. Use el cuadro de búsqueda para buscar **Entra** en la lista de conectores, busque **Microsoft Entra ID** y marque la casilla.

1. A la derecha, se abrirá un icono de vista previa.  Seleccione **Instalar**.

1. Una vez finalizada la instalación, selecciona el elemento de menú **Conectores de datos** en el menú Configuración.

    **Nota**: deberías ver Conector 1 instalado y **Microsoft Entra ID** en la lista.

1. Selecciona **Microsoft Entra ID** y después selecciona **Abrir página del conector**.

1. En la página del conector, se proporcionarán las instrucciones y los pasos siguientes para el conector de datos. Comprueba que hay una marca de verificación junto a cada uno de los **Requisitos previos** para continuar con la **Configuración**.

1. En **Configuración**, activa las casillas **Registros de inicio de sesión** y **Registros de auditoría**. Hay orígenes de registro adicionales disponibles, pero actualmente están en **Versión preliminar** y fuera del ámbito de este curso.

1. Seleccione **Aplicar cambios**. 

1. Se proporcionará una notificación de que los cambios se aplicaron correctamente. Ve al área de trabajo de **Microsoft Sentinel** seleccionando la **X** en la parte superior derecha de la página del conector.

1. Selecciona **Actualizar** en el icono **Microsoft Sentinel | Conectores de datos** y el número 1 se mostrará en el recuento **Conectados**.

   **Nota**: el conector de datos de Microsoft Entra ID puede tardar unos minutos en mostrarse en el recuento activo. 

#### Tarea 3: ejecutar consultas de Kusto en la actividad de usuario

1. En **Microsoft Sentinel**, ve a **Registros** en el título del menú **General**.

1. Cierra la ventana **Bienvenido a Log Analytics**.

1. Se abrirá una ventana con consultas de ejemplo. Selecciona **Auditar** y busca **ID de usuario**.

1. Seleccione **Ejecutar**. 

1. Esto proporcionará una lista de identificadores de usuario en Microsoft Entra ID.  Dado que acabamos de crear el área de trabajo, es posible que no veas los resultados.  Anota el formato de la consulta.
