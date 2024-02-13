---
lab:
  title: "10: Autenticación de Azure\_AD para máquinas virtuales de Windows y Linux"
  learning path: '02'
  module: Module 02 - Implement an Authentication and Access Management Solution
---

# Laboratorio 10: Autenticación de Microsoft Entra para máquinas virtuales de Windows y Linux

**Nota:** este laboratorio requiere un pase para Azure. Consulta el laboratorio 00 para obtener instrucciones.

## Escenario del laboratorio

La empresa ha decidido que Azure Active Directory debe usarse para iniciar sesión en máquinas virtuales para el acceso remoto.  En este laboratorio se muestra cómo se puede configurar para máquinas virtuales de Windows y Linux.

#### Tiempo estimado: 30 minutos

### Ejercicio 1: iniciar sesión en máquinas virtuales de Windows en Azure con Azure AD

#### Tarea 1: crear una máquina virtual de Windows con el inicio de sesión de Azure AD habilitado

1. Ve a [https://portal.azure.com](https://portal.azure.com)

1. Seleccione **+ Crear un recurso**.

1. Escribe **Windows 11** en el campo de búsqueda de la barra de búsqueda de Marketplace.

1. En el cuadro **Windows 11**, elige **Windows 11 Enterprise 22H2** en el menú desplegable Seleccionar un plan de software.

1. Tendrás que crear un nombre de usuario y una contraseña de administrador para la máquina virtual en la pestaña de conceptos básicos.
   - Usa un nombre de usuario que puedas recordar y una contraseña segura.

1. En la pestaña **Administración**, activa la casilla **Iniciar sesión con Azure AD** en la sección de Azure AD.

    NOTA: Desde el 11/1/2023 esta interfaz de usuario no se ha actualizado para mostrar Microsoft Entra ID, todavía hace referencia a Azure AD.

    NOTA 2: Observarás que la identidad** administrada asignada por el sistema** en la sección Identidad se activa automáticamente y se vuelve gris. Esta acción debe realizarse automáticamente una vez que se ha habilitado Login with Azure AD (Iniciar sesión con Azure AD).

1. Pase por el resto de la experiencia de creación de una máquina virtual. 

1. Seleccione Crear.

#### Tarea 2: iniciar sesión de Azure AD para Virtual Machines existentes de Azure

1. Ve a **Virtual Machines** en [https://portal.azure.com](https://portal.azure.com).

1. Selecciona la máquina virtual recién creada en la Tarea 1.

1. Seleccione **Access Control (IAM)** .

1. Selecciona **+ Agregar** y después **Agregar asignación de roles** para abrir la página Agregar asignación de roles.

1. Asigna la siguiente configuración:
    - **Tipo de asignación**: roles de función de trabajo.
    - **Rol**: inicio de sesión de administrador de Virtual Machine
    - **Miembros**: elige usuario, grupo o entidad de servicio.  Después usa **+ Seleccionar miembros** para agregar a **Joni Sherman** como usuario específico de la máquina virtual.

1. Selecciona **Revisar + asignar** para completar el proceso

#### Tarea 3: actualizar la máquina virtual del servidor para admitir el inicio de sesión de Azure AD

1. Selecciona el elemento del menú **Conectar**.

1. En la pestaña **RDP**, selecciona **Descargar archivo RDP**.  Si se te solicita, elige la opción **Mantener** para el archivo.  Se guardará en la carpeta Descargas.

1. Abre la carpeta **Descargas** en el Administrador de archivos.

1. Abre el RDP.

1. Elige iniciar sesión como usuario alternativo.

1. Usa el nombre de usuario y la contraseña de administrador creados al configurar la máquina virtual.
   - Si se te solicita, di que sí para permitir el acceso a la máquina virtual o a la sesión RDP.

1. Espera a que el servidor esté abierto y que todo el software se cargue, como el panel de administrador del servidor.

1. Selecciona el **botón Inicio** de la máquina virtual.

1. Escribe **Panel de control** e inicia la aplicación del panel de control.

1. Selecciona **Sistema y seguridad** en la lista de valores.

1. En la configuración del **Sistema**, selecciona la opción **Permitir acceso remoto**.

1. En la parte inferior del cuadro de diálogo que se abre verás un **Escritorio remoto**.

1. **Desactiva** la casilla denominada**Permitir conexiones solo de equipos que ejecuten Escritorio remoto con Autenticación a nivel de red**.

1. Seleccione **Apply** (Aplicar) y, después, **OK** (Aceptar).

1. **Sal** de la sesión RDP de la máquina virtual.


#### Tarea 4: Modificación del archivo RDP para admitir el inicio de sesión de Microsoft Entra ID

1. Abre la carpeta **Descargas** en el administrador de archivos.

1. **Haga una copia** del archivo RDP y agregue **-EntraID** al final del nombre de archivo.

1. Edita la nueva versión del archivo RDP que acabas de copiar con Bloc de notas. Agrega estas dos líneas de texto a la parte inferior del archivo:
     ```
        enablecredsspsupport:i:0
        authentication level:i:2
     ```
 
 1. **Guarda** el archivo RDP.  Ahora deberías tener dos versiones del archivo:
      - <<virtual machine name>>.RDP
      - <<virtual machine name>>-EntraID.RDP

#### Tarea 5: Conexión a la máquina virtual Windows mediante el inicio de sesión de Microsoft Entra ID

1. Abra **<<virtual machine name>>-EntraID.RDP

1. Selecciona **Conectar** cuando se abra el diálogo.

1. En lugar de ver una pregunta sobre la cuenta de usuario para iniciar sesión, deberías recibir un mensaje para conectarte al equipo remoto.

1. Selecciona **Sí** en la parte inferior de la página.

1. La sesión de Escritorio remoto debería abrirse y mostrar la pantalla de inicio de sesión de Windows Server.  Se debería mostrar **Otro usuario** con el botón Aceptar.

1. Seleccione **Aceptar**.

1. En el cuadro de diálogo de inicio de sesión, escribe la siguiente información:
   - Nombre de usuario = **AzureAD\JoniS@<<your lab domainname>>
   - Contraseña = Introduce la contraseña proporcionada por tu proveedor de laboratorios

   NOTA: JoniS es el usuario al que concedemos acceso para iniciar sesión como administrador durante la Tarea 1.

1. Windows Server debe confirmar el inicio de sesión y abrirse en el panel de Administrador del servidor normal.

#### Tarea 6: Pruebas opcionales para explorar el inicio de sesión de Microsoft Entra ID

1. Comprueba que JoniS era el único usuario agregado al grupo de administradores.

1. Haga clic con el botón secundario del ratón en el botón INICIAR y, a continuación, seleccione **Administración de equipos** en el menú emergente.

1. Abre **Usuarios y grupos locales** y después ve a **Grupos, Administradores**.

1. Deberías ver **Azure\JoniSherman....** en la lista.

1. Compruebe si otros miembros de Microsoft Entra ID pueden iniciar sesión.

1. Sal de la sesión de escritorio remoto.

1. Vuelve a iniciar el archivo **<<server name>>-AzureAD.RDP**.

1. Intenta iniciar sesión como otros miembros de Azure AD, como AdeleV o AlexW o DiegoS.

1. Deberías observar que a cada uno de estos usuarios se le deniega el acceso.

### Ejercicio opcional 2: iniciar sesión en Linux Virtual Machines en Azure con Azure AD

#### Tarea 1: crear una máquina virtual de Linux con la identidad administrada asignada por el sistema

1. Ve a [https://portal.azure.com](https://portal.azure.com)

1. Seleccione **+ Crear un recurso**.

1. Busque **Ubuntu**.

1. Seleccione **Crear** en **Ubuntu Server 22.04 LTS**. Puede usar otros servidores Linux para este laboratorio de pruebas.

1. En la pestaña **Administración**, marca la casilla para habilitar **Inicio de sesión con Azure Active Directory (vista previa)**.

1. Asegúrese de que la opción **Identidad administrada asignada por el sistema** se haya marcado.

1. Pase por el resto de la experiencia de creación de una máquina virtual. Durante esta versión preliminar, tendrá que crear una cuenta de administrador con nombre de usuario y contraseña o clave pública SSH.

#### Tarea 2: iniciar sesión de Azure AD para Virtual Machines existentes de Azure

1. Ve a **Virtual Machines** en [https://portal.azure.com](https://portal.azure.com).

1. Seleccione **Access Control (IAM)** .

1. Selecciona Agregar > Agregar asignación de funciones para abrir la página Agregar asignación de funciones.

1. Asigne el siguiente rol. 
    - **Rol**: iniciar sesión de administrador de máquina virtual o iniciar sesión de usuario de máquina virtual
    - **Asignar acceso a**: usuario, grupo, entidad de servicio o identidad administrada

1. Para asignar roles, consulte Asignación de roles de Azure mediante Azure Portal.
