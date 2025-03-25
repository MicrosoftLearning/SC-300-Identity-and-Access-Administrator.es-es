---
lab:
  title: 01 - Administración de roles de usuario
  learning path: '01'
  module: Module 01 - Implement an Identity Management Solution
---

# Inquilinos de WWL: términos de uso

Si se te proporciona un inquilino porque estás realizando un curso dirigido por un instructor, ten en cuenta que ese inquilino está disponible únicamente como apoyo para los laboratorios interactivos del curso. Los inquilinos no deben compartirse ni usarse para otros fines que no sean los de los laboratorios interactivos. El inquilino usado en este curso es un inquilino de prueba y no se puede usar ni tener acceso a él después de que la clase haya terminado y no es apto para la extensión. Los inquilinos no se deben convertir a suscripciones de pago. Los inquilinos obtenidos como parte de este curso siguen siendo propiedad de Microsoft Corporation y nos reservamos el derecho de acceso y recuperación en cualquier momento.

# Dos opciones de inicio de sesión diferentes

Este laboratorio tiene dos opciones de inicio de sesión diferentes, que se usan para diferentes partes del laboratorio. Un estilo de inicio de sesión es para laboratorios que requieren recursos de Azure, el otro es para laboratorios que solo necesitan recursos de Microsoft Entra y Microsoft 365. Tipos de registro:

  - Inicio de sesión basado en recursos de Azure
  - Inicio de sesión del inquilino de Microsoft 365 + E5
      - Cuenta de administrador MOD

Se te indicará qué nombre de usuario debes usar en cada uno de los laboratorios.


# Laboratorio 01: Administración de roles de usuario

### Tipo de inicio de sesión = Inicio de sesión del inquilino de Microsoft 365 + E5

## Escenario del laboratorio

Tu empresa ha contratado recientemente a un nuevo empleado que realizará tareas como administrador de aplicaciones. Debes crear un nuevo usuario y asignarle el rol adecuado.

#### Tiempo estimado: 30 minutos

### Ejercicio 1: Creación de un nuevo usuario y prueba de sus derechos de administrador de aplicaciones

#### Tarea 1: Adición de un nuevo usuario

1. Inicia sesión en  [https://entra.microsoft.com](https://entra.microsoft.com) como Administrador global.
 - Usa la cuenta **Administración de Microsoft 365**.

2. En el menú de la izquierda selecciona **Identidad**.

3. En el menú de navegación de la izquierda, en **Usuarios**, selecciona **Todos los usuarios** y después **+ Nuevo usuario** y **Crear nuevo usuario**.

4. Marca el botón **Crear usuario**. Después, crea un usuario con la siguiente información:

    | **Configuración**| **Valor**|
    | :--- | :--- |
    | Nombre principal de usuario| ChrisG|
    | Nombre| Chris Green|

5. Marca la opción **Generar contraseña automáticamente**.

6. Copia la contraseña generada en una ubicación donde puedas recordarla para la siguiente tarea.

     *Tendrás que cambiar la contraseña la primera vez que accedas a esta cuenta*

7. Selecciona **Revisar + crear**. Después, selecciona **Crear** en la pantalla de revisión. Al hacerlo, el usuario se crea y se registra en la organización.

#### Tarea 2: Inicio de sesión e intento de creación de una aplicación

1. Inicia una nueva ventana del explorador de InPrivate.
2. Abre el Centro de administración de Microsoft Entra [https://entra.microsoft.com](https://entra.microsoft.com) como Chris Green.

    | **Configuración**| **Valor**|
    | :--- | :--- |
    | Nombre de usuario| ChrisG@`your domain name.com`|
    | Contraseña| Escribe la contraseña generada automáticamente de la tarea anterior. |

3. Actualiza tu contraseña.

    | **Configuración**| **Valor**|
    | :--- | :--- |
    | Contraseña actual| Usa una contraseña generada automáticamente|
    | New Password| Escribe una contraseña única y segura |
    | Confirm Password| Vuelve a introducir una contraseña única y segura |

4. Busca y selecciona **Aplicaciones empresariales** en el cuadro de diálogo de búsqueda de la parte superior de la pantalla.

5. Selecciona **+ Nueva aplicación**. Ten en cuenta que **+ Crear tu propia aplicación** no está disponible.

6. Prueba seleccionando otra configuración como **Proxy de aplicación**, **Configuración de usuario** y otras para ver si **Chris Green** no tiene derechos.

7. Selecciona el nombre **ChrisG** en la esquina superior derecha y cierra la sesión.


### Ejercicio 2: Asignación de rol de administrador de aplicaciones y creación de una aplicación

#### Tarea 1: Asignación de un rol a un usuario

Con Microsoft Entra ID, puedes designar administradores limitados que administren tareas de identidad en roles con menos privilegios. Los administradores se pueden asignar para realizar tareas como agregar usuarios o cambiarlos, asignar roles administrativos, restablecer contraseñas de usuario, administrar licencias de usuario y administrar nombres de dominio.

1. Si aún no has iniciado sesión como Administrador global, abre el Centro de administración de Microsoft Entra e inicia sesión.
2. Ve a Identidad y selecciona la página Usuarios.
3. Selecciona **Todos los usuarios** en la sección Administrar del menú.
4. Selecciona la cuenta de **Chris Green**.
5. Elige **Roles asignados** en el menú Administrar.
6. Selecciona **+ Agregar asignaciones**.
7. Selecciona el rol `Application administrator` en la lista desplegable.
8. Selecciona el botón **Siguiente**.
9. Marca el valor **Activo** para **Tipo de asignación**.
10. Selecciona **Asignar**.

    ![Página Roles asignados, se muestra el rol seleccionado](./media/directory-role-select-role.png)

**Nota**: si el entorno de laboratorio ya ha activado Microsoft Entra ID Premium P2, se habilitará Privileged Identity Management (PIM) y tendrás que seleccionar **Siguiente** y asignar un Rol permanente a este usuario.

11. Selecciona el botón **Actualizar**.

**Nota: el rol de administrador de aplicaciones recién asignado aparece en la página de roles asignados del usuario.**

#### Tarea 2: Comprobación de los permisos de la aplicación

1. Inicia una nueva ventana del explorador de InPrivate.
2. Abre el Centro de administración de Microsoft Entra [https://entra.microsoftcom](https://entra.microsoft.com) como Chris Green.

    | **Configuración**| **Valor**|
    | :--- | :--- |
    | Nombre de usuario| ChrisG@`your domain name.com`|
    | Contraseña| Introduce la contraseña única y segura que has creado anteriormente |

3. Si aparece el cuadro de diálogo **Le damos la bienvenida a Microsoft Azure**, selecciona el botón **Quizás más tarde**.
4. Busca y selecciona **Aplicaciones empresariales** en el cuadro de diálogo de búsqueda en la parte superior de la pantalla.
5. Observa que **+ Nueva aplicación** está disponible ahora.
6. Selecciona **+ Nueva aplicación**
7. Observa que **+ Crear tu propia aplicación** no está en gris. Si eliges una aplicación de la galería, verás que el botón **Crear** está disponible.

   **Nota: este rol tiene ahora la capacidad de añadir aplicaciones al inquilino. Experimentaremos más con esta característica en laboratorios posteriores.**

8. Cierra la sesión de la instancia de Chris Green del portal y cierra el explorador.

### Ejercicio 3: Eliminación de una asignación de roles

#### Tarea 1: Eliminación del administrador de aplicaciones de Chris Green

Esta tarea usará un método alternativo para quitar el rol asignado; usará la opción **Roles y administradores** en Micrisoft Entra ID.

1. Si aún no has iniciado sesión como tu Administrador global, inicia el Centro de administración de Microsoft Entra e inicia sesión ahora.
2. En el cuadro de búsqueda, escribe **Roles** y después inicia los roles y la administración de Microsoft Entra ID.
3. En  **Todos los roles** de  **Roles y administradores**, selecciona el rol **Administrador de aplicaciones** de la lista.
4. En la página **Administrador de aplicaciones | Asignaciones** deberías ver el nombre Chris Green en la lista.
5. Desplázate hasta la derecha sobre Chris Green.
6. Selecciona **Quitar** de las opciones de la parte superior del diálogo.
7. Responde **Sí** cuando se abra el cuadro de confirmación.
8. Cierra la pantalla.

### Ejercicio 4: Importación masiva de usuarios

#### Tarea 1: Operaciones masivas para crear usuarios con un archivo .csv

1. En el menú de Microsoft Entra ID, abre primero **Identidad**, selecciona después **Usuarios** y selecciona a continuación **Todos los usuarios**.

2. En el mosaico **Usuarios | Todos los usuarios**, selecciona la flecha desplegable **Operaciones masivas** y después **Crear de forma masiva**.

3. Al seleccionar **Creación masiva** se abrirá un nuevo mosaico. Este mosaico proporciona un vínculo de **descarga** de un archivo de plantilla que editarás para rellenar con la información del usuario y cargar para agregar la creación masiva de usuarios.

4. Selecciona **Descargar** para descargar el archivo .csv.

5. La plantilla .csv te proporciona los campos incluidos con el perfil de usuario. Esto incluye el nombre de usuario, el nombre para mostrar y la contraseña inicial obligatorios. También puedes completar campos opcionales, como Ubicación y uso del departamento, en este momento. La captura de pantalla siguiente es un ejemplo de cómo puedes completar el archivo .csv: 

    ![Importación masiva con la entrada de archivo CSV](./media/bulkimportexample.png)

    Puedes modificar este archivo para agregar usuarios de forma masiva.  Observa que no es necesario rellenar todo el campo.  Según los datos de ejemplo proporcionados, debes agregar principalmente el nombre y la información de nombre de usuario.

6. Se ha proporcionado un CSV de ejemplo en la carpeta Allfiles/Labs/Lab1 folder -- **SC300BulkUser.csv**.
   1. Abre el Bloc de notas.
     - Dentro del entorno de laboratorio, selecciona el botón INICIAR y escribe Bloc de notas.  
   1. Abre el archivo SC300BulkUser.csv
   1. Cambia el valor de **escriba el nombre de dominio** en el dominio de tu entorno de laboratorio de Azure.
   1. Guarda el archivo.

7. En el diálogo **Creación masiva de usuarios**, selecciona el icono de carpeta de archivos del paso 3.

8. Accede mediante la ruta de acceso a la carpeta Allfiles/Labs/Lab1 y selecciona el archivo **SC300BulkUser.csv**.

9. Selecciona **Abrir**.

7. Se te notificará que el archivo se ha cargado correctamente.Elige **Enviar** para agregar los usuarios. 

Una vez creados los usuarios, se te comunicará que la creación se ha realizado correctamente.  Cierra el icono Creación masiva de usuarios y los nuevos usuarios se rellenarán en la lista **Usuarios | Todos los usuarios**. 

#### Tarea 2: Adición de usuarios de manera masiva con PowerShell

1. Abre PowerShell como administrador.Para ello, busca PowerShell en Windows y elige Ejecutar como administrador. 

**Nota:** debes tener PowerShell versión 7.2 o posterior para que este laboratorio funcione.  Cuando se abra PowerShell, verás una versión en la parte superior de la pantalla. Si ejecutas una versión anterior, sigue las instrucciones de la pantalla para ir a https://aka.ms/PowerShell-Release?tag=7.3.9. Desplázate hacia abajo hasta la sección activos y selecciona powershell-7.3.1-win-x64.msi. Cuando se haya completado la descarga, selecciona Abrir archivo. Instala con todos los valores predeterminados.

**Sugerencia de laboratorio**: TouchType no funciona bien con PowerShell en el entorno de laboratorio.  Para solucionar este problema, abre el Bloc de notas en el entorno de laboratorio. A continuación, usa la característica TouchType para colocar el script en el Bloc de notas y, después, usa Copiar y pegar para colocar el comando en PowerShell.  Disculpas por este paso adicional.

2. Deberás instalar el módulo de PowerShell Microsoft.Graph si no lo has usado antes.  Ejecuta los dos comandos siguientes y cuando se te pida confirmación pulsa Y:

    ```
    Install-Module Microsoft.Graph
    ```
3. Confirma que el módulo Microsoft.Graph está instalado:

    ```
    Get-InstalledModule Microsoft.Graph
    ```
    

4. Después deberás iniciar sesión en la API de Microsoft Graph mediante la ejecución de:  

    ```
    Connect-MgGraph -Scopes "User.ReadWrite.All"
    ``` 
    Se abrirá el explorador Edge y se te pedirá que inicies sesión.  Usa la cuenta de administrador MOD para conectarte.  Acepta la solicitud de permisos y cierra la ventana del explorador.

5. Para verificar que tienes conexión y ver los usuarios existentes, ejecuta:  

    ``` 
    Get-MgUser 
    ```
    
7. Para asignar una contraseña temporal común a todos los usuarios nuevos, ejecuta el siguiente comando y reemplaza <Enter a complex Password> por la contraseña que quieras proporcionar a tus usuarios.  

    ``` 
    $PWProfile = @{
        Password = "<Enter a complex password you will>";
        ForceChangePasswordNextSignIn = $false
    }
    ```

8. Ya puedes crear un nuevo usuario.  El comando siguiente se rellenará con la información del usuario y se ejecutará.  Si tienes más de un usuario que agregar, puedes usar un archivo TXT de Bloc de notas para agregar la información del usuario y copiar o pegar en PowerShell. 

    ```
    New-MgUser `
        -DisplayName "New PW User" `
        -GivenName "New" -Surname "User" `
        -MailNickname "newuser" `
        -UsageLocation "US" `
        -UserPrincipalName "newuser@<labtenantname.com>" `
        -PasswordProfile $PWProfile -AccountEnabled `
        -Department "Research" -JobTitle "Trainer"
    ```
**Nota**: reemplaza **labtenantname.com** por el nombre de **onmicrosoft.com** asignado por el inquilino del laboratorio.

## Experimentar con la administración de usuarios

Puedes agregar y quitar usuarios con la página Microsoft Entra ID.  Sin embargo, los usuarios se pueden crear y los roles se pueden asignar mediante el scripting.  Experimenta con dar a la cuenta de usuario de Chris Green un rol diferente con script. 
 

### Ejercicio 5: Eliminación de un usuario de Microsoft Entra ID

#### Tarea 1: Eliminación de un usuario

Puede ocurrir que se elimine una cuenta y después se deba recuperar. Debes comprobar que puedes recuperar una cuenta que se ha eliminado recientemente.

1. Ve a [https://entra.micrososft.com](Microsoft Entra admin center).

2. En el panel de navegación izquierdo, en **Identidad**, selecciona **Usuarios**.

3. Abre la lista **Todos los usuarios**, selecciona la casilla del usuario que se va a eliminar. Por ejemplo, selecciona **Chris Green**.

    **Sugerencia**: seleccionar usuarios de la lista te permite administrar varios usuarios al mismo tiempo. Si seleccionas un usuario y deseas abrir la página de ese usuario, solo administrarás a ese usuario.

    ![Imagen de pantalla que muestra la lista de usuarios Todos los usuarios con una casilla de usuario activada y otra resaltada, que indica la posibilidad de seleccionar varios usuarios de la lista.](./media/lp1-mod2-remove-user.png)

4. Con la cuenta de usuario seleccionada, en el menú, selecciona **Eliminar**.

5. Revisa el cuadro de diálogo y luego selecciona **Sí**.

#### Tarea 2: Restauración de un usuario eliminado

1. En la página Usuarios, selecciona **Todos los usuarios** en la navegación de la izquierda, selecciona **Usuarios eliminados**.

2. Revisa la lista de usuarios eliminados y selecciona **Chris Green**.

    **Importante**: de manera predeterminada, las cuentas de usuario eliminadas se quitan permanentemente de Azure Active Directory de manera automática después de 30 días.

3. En el menú, selecciona **Restaurar usuario**.

4. Revisa el cuadro de diálogo y, luego, selecciona **Aceptar**.

5. En el menú de navegación izquierdo, selecciona **Todos los usuarios**.

6. Comprueba que se restauró el usuario.


### Ejercicio 6: Adición de una licencia de Windows 10 a una cuenta de usuario

#### Tarea 1: Búsqueda del usuario sin licencia en Azure Active Directory

Algunas cuentas de usuario de tu organización no recibirán todos los productos disponibles en su licencia asignada o necesitarán actualizaciones o adiciones a su asignación de licencias. Debes asegurarte de que puedes actualizar la asignación de licencias de una cuenta de usuario en Microsoft Entra ID.

1. Ve a [https://entra.microsoft.com]( https://entra.microsoft.com).

2. En la navegación de la izquierda, en **Identidad**, selecciona **Usuarios** y después selecciona **Todos los usuarios**.

3. En la página Usuarios, escribe **Raul** en el cuadro de búsqueda.

4. Selecciona **Raul Razo**.

5. Revisa el perfil de Azure y asegúrate de que tiene la Ubicación de uso establecida.

    **Advertencia**: para asignar una licencia a un usuario, el usuario debe tener asignada una ubicación de uso.

6. Selecciona el elemento de menú **Licencias** del menú de la izquierda.

7. Asegúrate de que Raul tiene "No se han encontrado asignaciones de licencia".

#### Tarea 2: Adición de una licencia de Windows a Raúl

Tienes que agregar y quitar licencias a través del Centro de administración de Microsoft 365. Este es un cambio relativamente nuevo.

1. Abre una nueva pestaña en tu explorador.

2. Conéctate al Centro de administración de Microsoft 365 en [https://admin.microsoft.com](https://admin.microsoft.com).

3. Inicia sesión con tu cuenta de administrador si se te solicita.

4. En el menú de la izquierda, selecciona **Facturación** y después selecciona **Licencias**.

5. Selecciona la licencia **Windows 10/11 Enterprise E3** de la lista.

6. Elige el elemento **+ Asignar licencias**.

7. Busca **Raul Razo** en la lista.

8. Una vez que hayas agregado a Raúl, selecciona **Asignar**.

9. Vuelve a la pestaña del explorador con el **Centro de administración Microsoft Entra** abierto.

10. Vuelve a **Todos los usuarios** en el menú de la izquierda, en **Identidad**, y selecciona **Usuarios**.

11. En la página Usuarios, selecciona **Raul Razo**.

12. En el panel de navegación izquierdo, selecciona **Licencias.**

13. Observa que se ha asignado la licencia.

14. Puedes salir de la pantalla de licencia.
