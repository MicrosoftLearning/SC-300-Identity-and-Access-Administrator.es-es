---
lab:
  title: 10- Autenticación de Microsoft Entra ID para máquinas virtuales Windows y Linux
  learning path: '02'
  module: Module 02 - Implement an Authentication and Access Management Solution
---

# Laboratorio 10: Autenticación de Microsoft Entra para máquinas virtuales de Windows y Linux

### Tipo de inicio de sesión = Inicio de sesión de recurso de Azure

## Escenario del laboratorio

La empresa ha decidido que Microsoft Entra ID debe usarse para iniciar sesión en máquinas virtuales para el acceso remoto.  En este laboratorio se muestra cómo se puede configurar para máquinas virtuales de Windows y Linux.

#### Tiempo estimado: 30 minutos

### Ejercicio 1: Inicio de sesión en Windows Virtual Machines en Azure con Microsoft Entra ID

#### Tarea 1: Creación de una máquina virtual Windows con el inicio de sesión de Microsoft Entra ID habilitado

1. Ve a [https://portal.azure.com](https://portal.azure.com)

**Sugerencia de laboratorio**: si se le pide que guarde sus credenciales, elija "Nunca".  Y cancele el recorrido, a menos que esta sea su primera vez en Azure Portal.

1. Selecciona **+ Crear un recurso**.

1. Escribe **Windows 11** en la barra de búsqueda Buscar en Marketplace y, después, **Entrar**.

1. Busque el cuadro **Microsoft Windows 11** y, a continuación, seleccione **Crear** y elija **Windows 11 Enterprise, versión 22H2** en el menú que se abre.

1. Crea la VM con los valores siguientes en la pestaña **Aspectos básicos**:

  | Campo | Valor que se usará |
  | :-- | :-- |
  | Suscripción | Acepta el valor predeterminado |
  | Grupo de recursos | Crear nuevo: rgEntraLogin |
  | Nombre de la máquina virtual | vmEntraLogin |
  | Region | *default* |
  | Opciones de disponibilidad | No se requiere redundancia de la infraestructura |
  | Tipo de seguridad | Estándar |
  | Tamaño | vCPU DC1s_v3 - 1 estándar, 8 GiB de memoria |
  | Nombre de usuario administrador | vmEntraAdmin |
  | Contraseña del administrador | Usa la proporcionada por el entorno de laboratorio o crea una contraseña segura que puedas recordar |
  | Licencias | Confirma que tienes licencia |

1. No necesitarás cambiar nada en las pestañas **Discos** o **Redes**, pero puedes revisar los valores.

1. En la pestaña **Administración**, active la casilla **Iniciar sesión con Microsoft Entra ID** en la sección Microsoft Entra ID.

        NOTE: You will notice that the **System assigned managed identity** under the Identity section is automatically checked and turned grey. This action should happen automatically once you enable Login with Microsoft Entra ID.

1. Pase por el resto de la experiencia de creación de una máquina virtual. 

1. Selecciona **Revisar + crear** y después elige **Crear**.

#### Tarea 2: Inicio de sesión de Microsoft Entra ID para Azure Virtual Machines existente

1. Ve a **Virtual Machines** en [https://portal.azure.com](https://portal.azure.com).

1. Selecciona la máquina virtual recién creada en la Tarea 1.

1. Seleccione **Access Control (IAM)** .

1. Selecciona **+ Agregar** y después **Agregar asignación de roles** para abrir la página Agregar asignación de roles.

1. Asigna la siguiente configuración:
  - **Rol de función de trabajo**
  - **Rol**: inicio de sesión de administrador de Virtual Machine
  - **Miembros**: elige usuario, grupo o entidad de servicio.  Después usa **+ Seleccionar miembros** para agregar a **User2** como usuario específico de la máquina virtual.

1. Selecciona **Revisar + asignar** para completar el proceso.

#### Tarea 3: Actualización de la máquina virtual para permitir el inicio de sesión de Microsoft Entra ID

1. Selecciona el elemento del menú **Conectar**.

1. En la pestaña **RDP**, selecciona **Descargar archivo RDP**.  Si se te solicita, elige la opción **Mantener** para el archivo.  Se guardará en la carpeta Descargas.

1. Abre la carpeta **Descargas** en el Administrador de archivos.

1. Abre el RDP.

1. Elige iniciar sesión como usuario alternativo.

1. Usa el nombre de usuario y la contraseña de administrador (vmEntraAdmin) que has creado al configurar la máquina virtual.
   - Si se te solicita, di que sí para permitir el acceso a la máquina virtual o a la sesión RDP.

1. Espera a que se abra la máquina virtual y se cargue todo el software.

1. Selecciona el **botón Inicio** de la máquina virtual.

1. Escribe **Panel de control** e inicia la aplicación del panel de control.

1. Selecciona **Sistema y seguridad** en la lista de valores.

1. En la configuración del **Sistema**, selecciona la opción **Permitir acceso remoto**.

1. En la parte inferior del cuadro de diálogo que se abre verás un **Escritorio remoto**.

1. **Desactiva** la casilla denominada**Permitir conexiones solo de equipos que ejecuten Escritorio remoto con Autenticación a nivel de red**.

1. Selecciona **Aplicar** y, después, **Aceptar**.

1. **Sal** de la sesión RDP de la máquina virtual.

#### Tarea 4: Modificación del archivo RDP para admitir el inicio de sesión de Microsoft Entra ID

1. Abre la carpeta **Descargas** en el administrador de archivos.

1. **Haz una copia** del archivo RDP y agrega **-EntraID** al final del nombre de archivo.

1. Edita la nueva versión del archivo RDP que acabas de copiar con **Bloc de notas**. Agrega estas dos líneas de texto a la parte inferior del archivo:
     ```
        enablecredsspsupport:i:0
        authentication level:i:2
     ```
 
 1. **Guarda** el archivo RDP.  Ahora deberías tener dos versiones del archivo:
      - <<virtual machine name>>.RDP
      - <<virtual machine name>>-EntraID.RDP

#### Tarea 5: Conexión a la máquina virtual Windows mediante el inicio de sesión de Microsoft Entra ID

1. Abre **<<virtual machine name>>-EntraID.RDP

1. Selecciona **Conectar** cuando se abra el diálogo.

1. En lugar de ver una pregunta sobre la cuenta de usuario para iniciar sesión, deberías recibir un mensaje para conectarte al equipo remoto.

1. Selecciona **Sí** en la parte inferior de la página.

1. La sesión de Escritorio remoto debería abrirse y mostrar la pantalla de inicio de sesión de Windows Server.  Se debería mostrar **Otro usuario** con el botón Aceptar.

1. Selecciona **Aceptar**.

1. En el cuadro de diálogo de inicio de sesión, escribe la siguiente información:
   - Nombre de usuario = **AzureAD\User2@ tu nombre de dominio**
   - Contraseña = Introduce la contraseña proporcionada por tu proveedor de laboratorios

   NOTA: User2 es el usuario al que concedemos acceso para iniciar sesión como administrador durante la Tarea 1.

1. Windows debe confirmar el inicio de sesión y abrirlo en el escritorio normal.

#### Tarea 6: Pruebas opcionales para explorar el inicio de sesión de Microsoft Entra ID

1. Comprueba que User2 era el único usuario agregado al grupo de administradores.

1. Haz clic con el botón secundario del ratón en el botón INICIAR y, a continuación, selecciona **Administración de equipos** en el menú emergente.

1. Abre **Usuarios y grupos locales** y después ve a **Grupos, Administradores**.

1. Deberías ver **Azure\User2....** en la lista.

1. Comprueba si otros miembros de Microsoft Entra ID pueden iniciar sesión.

1. Sal de la sesión de escritorio remoto.

1. Vuelve a iniciar el archivo **<<server name>>-EntraID.RDP**.

1. Intenta iniciar sesión como otros miembros de Microsoft Entra ID.

1. Deberías observar que a cada uno de estos usuarios se le deniega el acceso.

### Ejercicio opcional 2: Inicio de sesión en Máquinas virtuales Linux en Azure con Microsoft Entra ID

#### Tarea 1: Creación de una máquina virtual de Linux con la identidad administrada asignada por el sistema

1. Ve a [https://portal.azure.com](https://portal.azure.com)

1. Selecciona **+ Crear un recurso**.

1. Busca **Ubuntu**.

1. Selecciona **Crear** en **Ubuntu Server 22.04 LTS**. Puedes usar otros servidores Linux para este laboratorio de pruebas.

1. En la pestaña**Administración**, activa la casilla para habilitar **Inicio de sesión con Microsoft Entra ID**.

1. Asegúrate de que la opción **Identidad administrada asignada por el sistema** se haya marcado.

1. Pasa por el resto de la experiencia de creación de una máquina virtual. Durante esta versión preliminar, tendrás que crear una cuenta de administrador con nombre de usuario y contraseña o clave pública SSH.

#### Tarea 2: Inicio de sesión de Microsoft Entra ID para Azure Virtual Machines existente

1. Ve a **Virtual Machines** en [https://portal.azure.com](https://portal.azure.com).

1. Selecciona **Access Control (IAM)**.

1. Selecciona Agregar > Agregar asignación de funciones para abrir la página Agregar asignación de funciones.

1. Asigna el siguiente rol. 
    - **Rol**: iniciar sesión de administrador de máquina virtual o iniciar sesión de usuario de máquina virtual
    - **Asignar acceso a**: usuario, grupo, entidad de servicio o identidad administrada

##### Parte de desafío del laboratorio

Intenta completar el resto de este laboratorio por tu cuenta. Es muy similar a la versión de Windows. Si buscas pasos detallados, consulta Asignación de roles de Azure mediante Azure Portal en Learn Docs.

