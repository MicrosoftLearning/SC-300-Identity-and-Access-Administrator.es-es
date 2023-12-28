---
lab:
  title: 27 - Consultas de Microsoft Sentinel Kusto para orígenes de datos de Azure AD
  learning path: '04'
  module: Module 04 - Plan and Implement and Identity Governance Strategy
---

# Laboratorio 27: consultas de Microsoft Sentinel Kusto para orígenes de datos de Azure AD

**Nota:** este laboratorio requiere un pase para Azure. Consulta el laboratorio 00 para obtener indicaciones.

## Escenario del laboratorio

Microsoft Sentinel es una solución SIEM y SOAR nativa de la nube de Microsoft.  Conectando orígenes de datos desde Microsoft y soluciones de seguridad de terceros, tienes la capacidad de ejecutar tareas de operaciones de seguridad.  En este ejercicio de laboratorio, crearás un área de trabajo de Microsoft Sentinel con conectores de datos en Azure AD para ejecutar consultas de búsqueda con Lenguaje de consulta Kusto (KQL). 

#### Tiempo estimado: 30 minutos

### Ejercicio 1: configuración de Microsoft Sentinel para consultas de Kusto

#### Tarea 1: crear un área de trabajo de Microsoft Sentinel

1. Inicia sesión en  [https://portal.azure.com](https://portal.azure.com) como Administrador global.

1. Busca y selecciona **Microsoft Sentinel**. 

1. Seleccione **Create Microsoft Sentinel** (Crear Microsoft Sentinel).

1. En el mosaico **Agregar Microsoft Sentinel a un área de trabajo**, selecciona **Crear una nueva área de trabajo**.

1. En **Grupo de recursos**, selecciona **Crear nuevo** e introduce **Sentinel-RG**.

1. Asigna un nombre al área de trabajo.  Ejemplo: SentinelLogAnalytics.

1. Seleccione una Región cercana.

1. Seleccione **Revisar y crear** y, a continuación, **Crear**.

1. Una vez finalizada la implementación del área de trabajo de Log Analytics, selecciona el botón **Actualizar**. Después selecciona tu área de trabajo y selecciona **Agregar**.  Esto agregará el área de trabajo a Microsoft Sentinel y abrirá Microsoft Sentinel.

1. Si se te solicita, selecciona **Aceptar** para activar la evaluación gratuita de Microsoft Sentinel.

#### Tarea 2: agregar Azure AD como origen de datos

1. En **Microsoft Sentinel**, navega en el menú hasta **Configuración** y selecciona **Conectores de datos**.

1. En la lista Conectores de datos, busca y selecciona **Azure Active Directory**.

1. A la derecha, se abrirá un mosaico de vista previa.  Seleccione **Open connector page** (Abrir página del conector).

1. En la página del conector, se proporcionarán las instrucciones y los pasos siguientes para configurar el conector de datos. Comprueba que una marca de verificación esté junto a cada uno de los **Requisitos previos** para continuar con la **Configuración**.

1. En **Configuración**, activa las casillas de los **Registros de inicio de sesión** y los **Registros de auditoría**. Hay orígenes de registro adicionales disponibles, pero actualmente están en **versión preliminar** y fuera del ámbito de este curso.

1. Seleccione **Aplicar cambios**. 

1. Se proporcionará una notificación de que los cambios se aplicaron correctamente. Ve al área de trabajo de **Microsoft Sentinel** y selecciona la **X** en la parte superior derecha de la página del conector.

1. Selecciona **Actualizar** en el mosaico **Microsoft Sentinel | Conectores de datos**, y verás el número 1 en el recuento de **Conectado**.

   **Nota:** el conector de datos de Azure AD puede tardar unos minutos en mostrarse en el recuento activo. 

#### Tarea 3: ejecutar consultas de Kusto en la actividad del usuario

1. En **Microsoft Sentinel**, ve a **Registros** en el encabezado de menú **General**.

1. Cierra la ventana **Le damos la bienvenida a Log Analytics**.

1. Se abrirá una ventana con consultas de muestra, selecciona **Auditar** y desplázate para buscar el **Id. de usuario**.

1. Seleccione **Ejecutar**. 

1. Esto proporcionará una lista de Id. de usuario en Azure AD.  Dado que acabamos de crear el área de trabajo, es posible que no veas los resultados.  Observa el formato de la consulta.

1. En **Administración de amenazas** en el menú, selecciona **Búsqueda**. 

1. Desplázate hacia abajo para buscar la consulta **Ubicación de inicio de sesión anómala por cuenta de usuario y aplicación de autenticación**.  Esta consulta sobre el inicio de sesión de Azure Active Directory sopesa todos los inicios de sesión de usuario para cada aplicación de Azure Active Directory y elige el cambio más anómalo en el perfil de ubicación de un usuario dentro de una aplicación individual. La intención es buscar el incidente de la cuenta de usuario, posiblemente a través de un vector de aplicación específico. 

1. Selecciona **Ver resultados de la consulta** para ejecutar la consulta.

1. Esto puede no proporcionar resultados con el nuevo área de trabajo, pero ahora has visto cómo se pueden ejecutar consultas para recopilar información o para buscar posibles amenazas.
