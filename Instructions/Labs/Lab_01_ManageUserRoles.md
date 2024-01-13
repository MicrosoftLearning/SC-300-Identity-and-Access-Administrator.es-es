---
lab:
  title: 01 - Administración de roles de usuario
  learning path: '01'
  module: Module 01 - Implement an Identity Management Solution
---

# Inquilinos de WWL: términos de uso
Si se le proporciona un inquilino porque está realizando un curso dirigido por un instructor, tenga en cuenta que ese inquilino está disponible únicamente como apoyo para los laboratorios prácticos del curso. Los inquilinos no deben compartirse ni usarse para otros fines que no sean los de los laboratorios prácticos. El inquilino usado en este curso es un inquilino de prueba y no se puede usar ni tener acceso a él después de que la clase haya terminado y no sea apto para la extensión. Los inquilinos no se deben convertir a suscripciones de pago. Los inquilinos obtenidos como parte de este curso siguen siendo propiedad de Microsoft Corporation y nos reservamos el derecho de acceso y recuperación en cualquier momento. 



# Laboratorio 1: administrar roles de usuario

## Escenario del laboratorio

Tu empresa ha contratado recientemente a un nuevo empleado que realizará tareas como administrador de aplicaciones. Debes crear un nuevo usuario y asignarle el rol adecuado.

#### Tiempo estimado: 30 minutos

### Ejercicio 1: creación de un nuevo usuario y prueba de sus derechos de administrador de aplicaciones

#### Tarea 1: agregar un nuevo usuario

1. Inicia sesión en  [https://portal.azure.com](https://portal.azure.com)  como administrador global.

2. Busque y seleccione **Azure Active Directory**.

3. En el menú de navegación de la izquierda, en **Administrar**, selecciona **Usuarios** y después **+ Nuevo usuario** y **Crear nuevo usuario**.

4. Marca el botón **Crear usuario**. Después, crea un usuario con la siguiente información:

    | **Configuración**| **Valor**|
    | :--- | :--- |
    | Nombre de usuario| ChrisG|
    | Nombre| Chris Green|
    | Nombre| Chris|
    | Apellido| Verde|

5. Marca la opción **Generar automáticamente la contraseña**.

6. Copia la contraseña generada en una ubicación donde puedas recordarla para la siguiente tarea.

     *Tendrás que cambiar la contraseña al iniciar sesión por primera vez en esta cuenta*

7. Seleccione **Crear**. Al hacerlo, el usuario se crea y se registra en la organización.

#### Tarea 2: iniciar sesión e intentar crear una aplicación

1. Inicia una nueva ventana del explorador de InPrivate.
2. Abre Azure Portal [https://portal.azure.com](https://portal.azure.com) como Chris Green.

    | **Configuración**| **Valor**|
    | :--- | :--- |
    | Nombre de usuario| ChrisG@`your domain name.com`|
    | Contraseña| Escribe la contraseña generada automáticamente de la tarea anterior. |

3. Actualiza tu contraseña.

    | **Configuración**| **Valor**|
    | :--- | :--- |
    | Contraseña actual| Usa una contraseña generada automáticamente|
    | New Password| Escribe una contraseña única y segura |
    | Confirm Password| Vuelve a escribir una contraseña única y segura |

4. Si aparece el cuadro de diálogo **Le damos la bienvenida a Microsoft Azure**, selecciona el botón **Quizás más tarde**.

5. Busca y selecciona **Aplicaciones empresariales** en el cuadro de diálogo de búsqueda en la parte superior de la pantalla.
7. Selecciona **+ Nueva aplicación**. Ten en cuenta que **+ Crear tu propia aplicación** no está disponible.

9. Prueba seleccionando otras opciones como **Proxy de aplicación**, **Configuración de usuario** y otras para ver si **Chris Green** no tiene derechos.
10. Selecciona el nombre **ChrisG** en la esquina superior derecha y cierra la sesión.


### Ejercicio 2: asignación de rol de administrador de aplicaciones y creación de una aplicación

#### Tarea 1: asignar un rol a un usuario

Con Azure Active Directory (Azure AD), puede designar administradores limitados que administren tareas de identidad en roles con menos privilegios. Los administradores se pueden asignar para realizar tareas como agregar usuarios o cambiarlos, asignar roles administrativos, restablecer contraseñas de usuario, administrar licencias de usuario y administrar nombres de dominio.

1. Si aún no has iniciado sesión con el rol de Administrador global, abre Azure Portal e inicia sesión.
2. Ve a la página de Azure Active Directory.
3. Selecciona **Usuarios** en la sección Gestionar del menú.
4. Selecciona la cuenta **Chris Green**.
5. Elige **Roles asignados** en el menú Administrar.
6. Selecciona **+ Agregar asignaciones** y marca la función `Application administrator`.
7. Seleccione **Agregar**.

    ![Página Roles asignados, se muestra el rol seleccionado](./media/directory-role-select-role.png)

**Nota:** si el entorno de laboratorio ya ha activado Azure AD Premium P2, Privileged Identity Management (PIM) se habilitará, y deberás seleccionar **Siguiente** y asignar un rol permanente a este usuario.

8. Selecciona el botón **Actualizar**.

**Nota: el rol de administrador de aplicaciones recién asignado aparece en la página de roles asignados del usuario.**

#### Tarea 2: comprobar los permisos de la aplicación

1. Inicia una nueva ventana del explorador de InPrivate.
2. Abre Azure Portal [https://portal.azure.com](https://portal.azure.com) como Chris Green.

    | **Configuración**| **Valor**|
    | :--- | :--- |
    | Nombre de usuario| ChrisG@`your domain name.com`|
    | Contraseña| Introduce la contraseña única y segura que has creado anteriormente |

3. Si aparece el cuadro de diálogo **Le damos la bienvenida a Microsoft Azure**, selecciona el botón **Quizás más tarde**.
4. Busca y selecciona **Aplicaciones empresariales** en el cuadro de diálogo de búsqueda en la parte superior de la pantalla.
5. Observa que **+ Nueva aplicación** está disponible ahora.
6. Selecciona **+ Nueva solicitud**

   **Nota: este rol ahora tiene la capacidad de agregar aplicaciones al inquilino. Experimentaremos más con esta característica en laboratorios posteriores.**

7. Cierra la sesión de la instancia de Chris Green de Azure Portal y cierra el explorador.

### Ejercicio 3: eliminación de una asignación de roles

#### Tarea 1: eliminar el administrador de aplicaciones de Chris Green

Esta tarea usará un método alternativo para quitar el rol asignado; usarás la opción **Roles y administradores** en Azure AD.

1. Si aún no has iniciado sesión como tu administrador global, abre Azure Portal e inicia sesión ahora.
2. En el cuadro de búsqueda, escribe **Azure Active Directory** e inicia Azure AD.
3. En  **Azure Active Directory**, selecciona  **Roles y administradores** y después selecciona el rol **Administrador de aplicaciones** de la lista.

**Nota:** si el entorno de laboratorio ya ha activado Azure AD Premium P2, Privileged Identity Management (PIM) se habilitará, y deberás seleccionar **Siguiente** y asignar un rol permanente a este usuario.

4. En la página **Administrador de aplicaciones | Asignaciones** deberías ver el nombre de Chris Green en la lista.
5. Coloca una marca de verificación en el cuadro junto a Chris Green.
6. Selecciona **X Quitar asignaciones** de las opciones de la parte superior del cuadro de diálogo.
7. Responde **Sí** cuando se abra el cuadro de confirmación.
8. Cierra Azure Active Directory.

### Ejercicio 4: importación de usuarios de manera masiva

#### Tarea 1: operaciones masivas para crear usuarios con un archivo .csv

1. En el menú de Azure AD, selecciona **Usuarios** en **Administrar**.

2. En el mosaico **Usuarios | Todos los usuarios**, selecciona la flecha desplegable **Operaciones masivas** y después, **Creación masiva**.

3. Al seleccionar **Creación masiva** se abrirá un nuevo mosaico. Este mosaico proporciona un vínculo de **descarga** de un archivo de plantilla que editarás para rellenar con la información del usuario y cargar para agregar la creación masiva de usuarios.

4. Selecciona **Descargar** para descargar el archivo .csv.

5. La plantilla .csv te proporciona los campos incluidos con el perfil de usuario. Esto incluye el nombre de usuario, el nombre para mostrar y la contraseña inicial obligatorios. También puede completar campos opcionales, como Ubicación y uso del departamento, en este momento. La captura de pantalla siguiente es un ejemplo de cómo puedes completar el archivo .csv: 

    ![Importación masiva con la entrada de archivo CSV](./media/bulkimportexample.png)

    Puedes modificar este archivo para agregar usuarios de forma masiva.  Observa que no es necesario rellenar todo el campo.  Según los datos de ejemplo proporcionados, debes agregar principalmente el nombre y la información de nombre de usuario.

6. Se ha proporcionado un CSV de ejemplo en la carpeta Allfiles/Lab1 -- **SC300BulkUser.csv**.
   1. Abra Bloc de notas.
     - Dentro del entorno de laboratorio, selecciona el botón INICIAR y escriba Bloc de notas.  
   1. Abre el archivo SC300BulkUser.csv
   1. Cambia el valor de **escriba el nombre de dominio** en el dominio de tu entorno de laboratorio de Azure.
   1. Guarde el archivo.

7. En el diálogo **Creación masiva de usuarios**, selecciona el icono de carpeta de archivos del paso 3.

8. Accede mediante la ruta de acceso a la carpeta Allfiles/Lab1 y selecciona el archivo **SC300BulkUser.csv**.

9. seleccione **Open**(Abrir).

7. Se te notificará que el archivo se ha cargado correctamente.Elige **Enviar** para agregar los usuarios. 

Una vez creados los usuarios, se te comunicará que la creación se ha realizado correctamente.  Cierra el icono Creación masiva de usuarios y los nuevos usuarios se rellenarán en la lista **Usuarios | Todos los usuarios**. 

#### Tarea 2: agregar usuarios de manera masiva con PowerShell

1. Abra PowerShell como administrador. Esto se puede hacer buscando PowerShell en Windows y eligiendo Ejecutar como administrador. 

**Nota:** selecciona PowerShell y no PowerShell ISE.

2. Deberás agregar e importar el módulo de PowerShell de Azure AD si no lo has usado antes.  Ejecuta los dos comandos siguientes y, cuando se te pida confirmación, pulsa Y:

    ```
    Install-Module AzureAD
    Import-Module AzureAD
    ```

3. Confirma que el módulo se ha instalado correctamente ejecutando el comando:  

    ```
    Get-Module AzureAD 
    ```

4. Después, tendrás que iniciar sesión en Azure ejecutando:  

    ```
    Connect-AzureAD 
    ``` 

5. La ventana de inicio de sesión de Microsoft aparecerá para que inicies sesión en Azure AD.  

6. Para comprobar que tienes conexión y para ver los usuarios existentes, ejecuta:  

    ``` 
    Get-AzureADUser 
    ```
    
7. Para asignar una contraseña temporal común a todos los usuarios nuevos, ejecuta el siguiente comando y reemplaza TempPW por la contraseña que deseas proporcionar a tus usuarios.  

    ``` 
    $PasswordProfile = New-Object -TypeName Microsoft.Open.AzureAD.Model.PasswordProfile
    ```

    ```
    $PasswordProfile.Password = "<<enter a secure password you will remember>>" 
    ```

8. Ya puedes crear nuevos usuarios.  El comando siguiente se rellenará con la información del usuario y se ejecutará.  Si tienes más de un usuario que agregar, puedes usar un archivo TXT de Bloc de notas para agregar la información del usuario y copiar o pegar en PowerShell. 

    ```
    New-AzureADUser -DisplayName "New User" -PasswordProfile $PasswordProfile -UserPrincipalName "NewUser@labtenantname.com" -AccountEnabled $true -MailNickName "Newuser"
    ```
**Nota**: reemplaza **labtenantname.com** por el nombre de **onmicrosoft.com** asignado por el inquilino del laboratorio.

## Experimenta con la administración de usuarios

Puedes agregar y quitar usuarios con la página de Azure AD.  Sin embargo, los usuarios se pueden crear y los roles se pueden asignar con scripting.  Experimenta dando a la cuenta de usuario Chris Green un rol diferente con scripts. 
 

### Ejercicio 5: eliminación de un usuario de Azure Active Directory

#### Tarea 1: eliminar un usuario

Puede ocurrir que se elimine una cuenta y después se deba recuperar. Debes comprobar que puedes recuperar una cuenta que se ha eliminado recientemente.

1. Vaya a [https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview]( https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview).

2. En el panel de navegación izquierdo, en **Administrar**, seleccione **Usuarios**.

3. En la lista **Usuarios**, active la casilla correspondiente al usuario que se va a eliminar. Por ejemplo, seleccione **Chris Green**.

    **Sugerencia**: seleccionar usuarios de la lista te permite administrar varios usuarios al mismo tiempo. Si selecciona un usuario y desea abrir la página de ese usuario, solo administrará a ese usuario.

    ![Imagen de pantalla que muestra la lista de usuarios Todos los usuarios con una casilla de usuario activada y otra resaltada, que indica la posibilidad de seleccionar varios usuarios de la lista.](./media/lp1-mod2-remove-user.png)

4. Con la cuenta de usuario seleccionada, en el menú, selecciona **Eliminar**.

5. Revisa el cuadro de diálogo y luego selecciona **Sí**.

#### Tarea 2: restaurar un usuario eliminado

1. En la página Usuarios, en el panel de navegación izquierdo, seleccione **Usuarios eliminados**.

2. Revisa la lista de usuarios eliminados y selecciona **Chris Green**.

    **Importante**: de manera predeterminada, las cuentas de usuario eliminadas se quitan permanentemente de Azure Active Directory de manera automática después de 30 días.

3. En el menú, seleccione **Restaurar usuario**.

4. Revise el cuadro de diálogo y, luego, seleccione **Aceptar**.

5. En el menú de navegación izquierdo, seleccione **Todos los usuarios**.

6. Compruebe que se restauró el usuario.


### Ejercicio 6: agregar una licencia de Windows 10 a una cuenta de usuario

#### Tarea 1: buscar el usuario sin licencia en Azure Active Directory

Algunas cuentas de usuario de su organización no recibirán todos los productos disponibles en su licencia asignada o necesitarán actualizaciones o adiciones a su asignación de licencias. Debes asegurarte de que puedes actualizar la asignación de licencias de una cuenta de usuario en Azure AD.

1. Vaya a [https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview]( https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview).

2. En el panel de navegación izquierdo, en **Administrar**, selecciona **Usuarios**.

3. En la página Usuarios, introduce **Raul** en el cuadro de búsqueda.

4. Selecciona **Raul Razo**.

5. Revisa el perfil de Azure y asegúrate de que tiene la Ubicación de uso establecida.

    **Advertencia**: para asignar una licencia a un usuario, el usuario debe tener asignada una ubicación de uso.

6. Selecciona la opción **Licencias** del menú de la izquierda.

7. Asegúrate de que Raul tenga marcada la opción "No se han encontrado asignaciones de licencia".

8. Vaya a [https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview]( https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview).

9. En el panel de navegación izquierdo, en **Administrar**, selecciona **Usuarios**.

10. En la página Usuarios, selecciona **Raul Razo**.

11. En el panel de navegación izquierdo, seleccione **Licencias.**

12. Selecciona el botón **+ Asignaciones**. 

13. En la página Actualizar asignaciones de licencias, activa la casilla de una licencia de **Windows 10/11 Enterprise E3**.

    ![Imagen de pantalla que muestra la página Actualizar asignaciones de licencia y las opciones de licencia resaltadas](./media/lp1-mod2-assign-user-license-options.png)

14. Cuando haya terminado, seleccione **Guardar**.

15. En la parte superior de la pantalla, selecciona **Inicio**, luego selecciona **Contoso**, **Usuario** y, finalmente, **Raul Razo**.

16. Observa que se ha asignado la licencia.
