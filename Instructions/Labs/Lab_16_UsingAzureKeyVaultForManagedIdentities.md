---
lab:
  title: '16: uso de Azure Key Vault para identidades administradas'
  learning path: '02'
  module: Module 02 - Implement an Authentication and Access Management Solution
---

# Laboratorio 16: usar Azure Key Vault para identidades administradas

**Nota:** este laboratorio requiere un pase para Azure. Consulta el laboratorio 00 para obtener indicaciones.

## Escenario del laboratorio

Al usar identidades administradas para recursos de Azure, el código puede obtener tokens de acceso para autenticarse en aquellos recursos que admitan la autenticación de Azure AD.Sin embargo, no todos los servicios de Azure son compatibles con la autenticación de Azure AD. Para usar identidades administradas para recursos de Azure con esos servicios, almacena las credenciales del servicio en Azure Key Vault y usa la identidad administrada para acceder a Key Vault y recuperar las credenciales.

#### Tiempo estimado: 20 minutos

### Ejercicio 1: usar Azure Key Vault para administrar identidades de máquina virtual

#### Tarea 1: crear una máquina virtual Windows

1. Inicia sesión en [https://portal.azure.com](https://portal.azure.com)

1. Seleccione **+ Crear un recurso**.

1. Escribe **cliente Windows** en la barra de búsqueda de Marketplace.

1. Selecciona **Cliente Windows** y, en el menú desplegable de planes, selecciona **Windows 10 Enterprise, versión 22H2 - x64 Gen 1**. Luego, elija **Crear**.

1. Tendrás que crear un nombre de usuario y una contraseña de administrador para la máquina virtual en la pestaña de conceptos básicos.

1. En la pestaña **Administración**, marca la casilla para **Activar la identidad administrada asignada al sistema**.

1. Pase por el resto de la experiencia de creación de una máquina virtual. 

1. Seleccione Crear.

#### Tarea 2: crear un almacén de claves

1. Inicia sesión en [https://portal.azure.com]( https://portal.azure.com) con una cuenta de administrador global.

1. En la parte superior de la barra de navegación izquierda, seleccione Crear un recurso.

1. En el cuadro Buscar del Marketplace, escribe **Key Vault**.  

1. En la lista de resultados, seleccione **Key Vault**.

1. Seleccione **Crear**.

1. Rellena toda la información necesaria como se indica a continuación. Asegúrate de elegir la suscripción que vas a usar para este laboratorio.
    **Nota**: el nombre de Key Vault debe ser único. Busca una marca de verificación verde a la derecha del campo.

 - **Grupo de recursos** - sc300KeyVaultrg
 - **Nombre del almacén de claves** - *anyuniquevalue*
 - En la página **Configuración de acceso**, selecciona el botón de radio **Directiva de acceso** del almacén.
1. Seleccione **Revisar + crear**.

1. Seleccione **Crear**.


#### Tarea 3: crear un secreto

1. Vaya al almacén de claves recién creado.

1. Seleccione **Secrets** (Secretos).

1. Seleccione **+ Generar/Importar**.

1. En la pantalla Crear un secreto desde Opciones de carga, deja la opción **Manual** seleccionada.

1. Escriba un nombre y un valor para el secreto.  El valor puede ser cualquiera de su elección. 

1. Deje la fecha de activación y la fecha de expiración y deje la opción Habilitado en Sí. 

1. Seleccione **Crear** para crear el secreto.

#### Tarea 4: conceder acceso a Key Vault

1. Vaya al almacén de claves recién creado.

1. Selecciona **Directivas de acceso** en el menú de la izquierda.

1. Seleccione **+ Create** (+ Crear).

1. En la sección Agregar directiva de acceso que se encuentra en Configurar a partir de una plantilla (opcional), elija Administración de secretos en el menú desplegable.

1. Para **Entidad de servicio**, elige **Ninguna seleccionada** para abrir la lista de entidades de servicio para seleccionar. En el campo de búsqueda, escribe el nombre de la máquina virtual que has creado en la tarea 2.  Seleccione la máquina virtual de la lista de resultados y elija Seleccionar.

1. Seleccione **Agregar**.

1. Seleccione **Guardar**.

#### Tarea 5: acceder a datos con el secreto de Key Vault con PowerShell

1. En la máquina virtual del laboratorio, abre PowerShell.  

1. En PowerShell, invoque la solicitud web en el inquilino para obtener el token del host local en el puerto específico de la máquina virtual.  

    ```
    $Response = Invoke-RestMethod -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net' -Method GET -Headers @{Metadata="true"}
    ```

1. Luego, extraiga el token de acceso de la respuesta.  

    ```
    $KeyVaultToken = $Response.access_token
    ```

1. Usa el comando Invoke-WebRequest de PowerShell para recuperar el secreto que has creado anteriormente en Key Vault y puntúa el token de acceso en el encabezado Autorización.  Necesitará la dirección URL de su almacén de claves, que se encuentra en la sección de Información esencial de la página Introducción del almacén de claves.  Recordatorio: el URI de Key Vault se encuentra en la Información general.

    ```
    Invoke-RestMethod -Uri https://<your-key-vault-URI>/secrets/<secret-name>?api-version=2016-10-01 -Method GET -Headers @{Authorization="Bearer $KeyVaultToken"}
    ```
1. Deberías recibir una respuesta similar a la siguiente: 
    ```
    'My Secret' https://mi-lab-vault.vault.azure.net/secrets/mi-test/50644e90b13249b584c44b9f712f2e51 @{enabled=True; created=16…
    ```
1. Este secreto se puede usar para autenticarse en servicios que requieren un nombre y una contraseña.
