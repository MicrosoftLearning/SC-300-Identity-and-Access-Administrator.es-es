---
lab:
  title: "10 - Autenticar Azure\_AD para máquinas virtuales Windows y Linux"
  learning path: '02'
  module: Module 02 - Implement an Authentication and Access Management Solution
---

# Laboratorio 10: autenticar Azure AD para máquinas virtuales Windows y Linux

**Nota:** este laboratorio requiere un pase para Azure. Consulta el laboratorio 00 para obtener indicaciones.

## Escenario del laboratorio

La empresa ha decidido que Azure Active Directory debe usarse para iniciar sesión en máquinas virtuales para el acceso remoto.  En este laboratorio se muestra cómo se puede configurar para máquinas virtuales Windows y Linux.

#### Tiempo estimado: 30 minutos

### Ejercicio 1: inicio de sesión en Windows Virtual Machines en Azure con Azure AD

#### Tarea 1: crear una máquina virtual de Windows con el inicio de sesión de Azure AD habilitado

1. Inicia sesión en [https://portal.azure.com](https://portal.azure.com)

1. Seleccione **+ Crear un recurso**.

1. Escriba **Windows Server** en el campo de búsqueda de la barra de búsqueda de Marketplace.

1. Selecciona **Windows Server** y elige **Windows Server 2019 Datacenter** en la lista desplegable Seleccionar un plan de software.

1. Tendrás que crear un nombre de usuario y una contraseña de administrador para la máquina virtual en la pestaña de conceptos básicos.
   - Usa un nombre de usuario que recordarás y una contraseña segura.

1. En la pestaña **Administración**, activa la casilla Iniciar sesión con Azure AD en la sección de Azure AD.

1. Observarás que **Identidad administrada asignada por el sistema** en la sección Identidad está activada automáticamente en gris. Esta acción debe realizarse automáticamente una vez que se ha habilitado Login with Azure AD (Iniciar sesión con Azure AD).

1. Pase por el resto de la experiencia de creación de una máquina virtual. 

1. Seleccione Crear.

#### Tarea 2: iniciar sesión de Azure AD para máquinas virtuales de Azure existentes

1. Ve a **Máquinas virtuales** en [https://portal.azure.com](https://portal.azure.com).

1. Selecciona la máquina virtual recién creada en la Tarea 1.

1. Seleccione **Access Control (IAM)** .

1. Selecciona **+ Agregar** y después **Agregar asignación de roles** para abrir la página Agregar asignación de roles.

1. Asigna la siguiente configuración:
    - **Tipo de asignación**: roles de función de trabajo
    - **Rol**: inicio de sesión del administrador de máquina virtual
    - **Miembros**: elige Usuario, grupo o entidad de servicio.  Después usa **+ Seleccionar miembros** para agregar **Joni Sherman** como usuario específico para la máquina virtual.

1. Selecciona **Revisar + asignar** para completar el proceso

#### Tarea 3: actualizar la máquina virtual del servidor para que sea compatible con el inicio de sesión de Azure AD

1. Selecciona el elemento de menú **Conectar**.

1. En la pestaña **RDP**, selecciona **Descargar archivo RDP**.  Si se te solicita, elige la opción **Mantener** para el archivo.  Se guardará en tu carpeta Descargas.

1. Abre la carpeta **Descargas** en el Administrador de archivos.

1. Abre RDP.

1. Elige iniciar sesión como usuario alternativo.

1. Usa el nombre de usuario de administrador y contraseña que has creado al configurar la máquina virtual.
   - Si el sistema lo solicita, responde de manera afirmativa para permitir el acceso a la máquina virtual o a la sesión RDP.

1. Espera a que se abra el servidor y se cargue todo el software, como el panel del Administrador del servidor.

1. Selecciona el **botón Iniciar** de la máquina virtual.

1. Escribe **Panel de control** e inicia la aplicación del panel de control.

1. Selecciona **Sistema y seguridad** en la lista de configuraciones.

1. En la configuración **Sistema**, selecciona la opción **Permitir acceso remoto**.

1. En la parte inferior del cuadro de diálogo que se abre verás la sección **Escritorio remoto**.

1. **Desactiva** la casilla **Permitir conexiones solo desde equipos que ejecutan Escritorio remoto con autenticación a nivel de red**.

1. Seleccione **Apply** (Aplicar) y, después, **OK** (Aceptar).

1. **Sal** de la sesión RDP de la máquina virtual.


#### Tarea 4: modificar tu archivo RDP para que sea compatible con el inicio de sesión de Azure AD

1. Abre la carpeta **Descargas** en el administrador de archivos.

1. **Haz una copia** del archivo RDP y agrega **-AzureAD** al final del nombre de archivo.

1. Edita la nueva versión del archivo RDP que acabas de copiar con el Bloc de notas. Agrega estas dos líneas de texto a la parte inferior del archivo:
     ```
        enablecredsspsupport:i:0
        authentication level:i:2
     ```
 
 1. **Guarda** el archivo RDP.  Ahora deberías tener dos versiones del archivo:
      - <<virtual machine name>>.RDP
      - <<virtual machine name>>-AzureAD.RDP

#### Tarea 5: conectarse al centro de datos de Windows Server 2022 con el inicio de sesión de Azure AD

1. Abre **<<virtual machine name>>-AzureAD.RDP

1. Selecciona **Conectar** cuando se abra el cuadro de diálogo.

1. En lugar de que el sistema te pregunte con qué cuenta de usuario iniciar sesión, debería preguntarte si quieres conectarte al equipo remoto.

1. Selecciona **Sí** en la parte inferior de la pantalla.

1. La sesión de Escritorio remoto debería abrirse y mostrar la pantalla de inicio de sesión de Windows Server.  Debería aparecer **otro usuario** con el botón Aceptar.

1. Seleccione **Aceptar**.

1. En el cuadro de diálogo de inicio de sesión, introduce la siguiente información:
   - Nombre de usuario = **AzureAD\JoniS@<<your lab domainname>>
   - Contraseña = Introduce la contraseña proporcionada por tu proveedor de laboratorios

   NOTA: JoniS es el usuario al que hemos concedido acceso para iniciar sesión como administrador durante la tarea 1.

1. Windows Server debe confirmar el inicio de sesión y abrir el panel Administrador del servidor normal.

#### Tarea 6: pruebas opcionales para explorar el inicio de sesión de Azure AD

1. Comprueba que JoniS era el único usuario agregado al grupo Administradores.

1. En el panel de Administrador del servidor, selecciona el menú **Herramientas** de la parte superior izquierda.

1. Inicia la herramienta **Administración de equipos**.

1. Abre **Usuarios y grupos locales** y después ve a **Grupos, administradores**.

1. Deberías ver **Azure\JoniSherman....** en la lista.

1. Comprueba si otros miembros de Azure AD pueden iniciar sesión.

1. Sal de la sesión de escritorio remoto.

1. Inicia de nuevo el archivo **<<server name>>-AzureAD.RDP**.

1. Intenta iniciar sesión como otros miembros de Azure AD, como AdeleV o AlexW o DiegoS.

1. Verás que a cada uno de estos usuarios se les deniega el acceso.

### Ejercicio opcional 2: inicio de sesión en máquinas virtuales Linux en Azure con Azure AD

#### Tarea 1: crear una máquina virtual GNU/Linux con la identidad administrada asignada por el sistema

1. Inicia sesión en [https://portal.azure.com](https://portal.azure.com)

1. Seleccione **+ Crear un recurso**.

1. Selecciona **Crear** en **Ubuntu Server 18.04 LTS** en la vista Popular.

1. En la pestaña **Administración**, marca la casilla para habilitar **Inicio de sesión con Azure Active Directory (vista previa)**.

1. Asegúrese de que la opción **Identidad administrada asignada por el sistema** se haya marcado.

1. Pase por el resto de la experiencia de creación de una máquina virtual. Durante esta versión preliminar, tendrá que crear una cuenta de administrador con nombre de usuario y contraseña o clave pública SSH.

#### Tarea 2: iniciar sesión de Azure AD para máquinas virtuales de Azure existentes

1. Ve a **Máquinas virtuales** en [https://portal.azure.com](https://portal.azure.com).

1. Seleccione **Access Control (IAM)** .

1. Selecciona Agregar > Agregar asignación de funciones para abrir la página Agregar asignación de funciones.

1. Asigne el siguiente rol. 
    - **Rol**: iniciar sesión de administrador de máquina virtual o iniciar sesión de usuario de máquina virtual
    - **Asignar acceso a**: usuario, grupo, entidad de servicio o identidad administrada

1. Para asignar roles, consulte Asignación de roles de Azure mediante Azure Portal.
