---
lab:
  title: 05 - Agregar usuarios invitados al directorio
  learning path: '01'
  module: Module 01 - Implement an identity management solution
---

# Laboratorio 05: agregar usuarios invitados al directorio

## Escenario del laboratorio

Tu empresa trabaja con muchos proveedores y, en ocasiones, debes agregar algunas cuentas de proveedor al directorio como invitado.

#### Tiempo estimado: 20 minutos

### Ejercicio 1: agregar usuarios invitados al directorio

#### Tarea: agregar el usuario invitado

1. Inicia sesión en  [https://portal.azure.com](https://portal.azure.com)  como usuario que tiene asignado un rol de directorio administrador limitado o el rol de invitador de usuarios invitados.

2. Selecciona  **Azure Active Directory**.

3. En  **Administrar**, selecciona  **Usuarios**.

4. Selecciona  **+ Nuevo usuario**.

5. En la página Nuevo usuario, selecciona **Invitar a usuario externo** y después agrega tu información como usuario invitado.

    **NOTA**: no se admiten direcciones de correo electrónico grupales. Escribe la dirección de correo electrónico de una persona. Además, algunos proveedores de correo electrónico permiten a los usuarios agregar un signo más (+) y texto adicional a sus direcciones de correo electrónico, ya que ello sirve de ayuda a algunas funciones como el filtrado de la bandeja de entrada. Sin embargo, Azure AD no admite actualmente signos más (+) en las direcciones de correo electrónico. Para evitar problemas de entrega, omita el signo más y los caracteres siguientes hasta el símbolo @.

6. Escribe una dirección de correo electrónico como **sc300externaluser1@sc300email.com**.

7. Cuando haya terminado, seleccione **Invitar**.

8. En la pantalla de Usuarios, comprueba que aparece tu cuenta y, en la columna **Tipo de usuario**, comprueba que aparece **Invitado**.

Después de enviar la invitación, la cuenta de usuario se agrega automáticamente al directorio como invitado.


### Ejercicio 2: invitación masiva a usuarios invitados

#### Tarea 1: invitar usuarios de forma masiva

Se ha establecido una asociación reciente con otra empresa. Por ahora, los empleados de la empresa asociada se agregarán como invitados. Debes asegurarte de que puedes importar varios usuarios invitados a la vez.

1. Inicia sesión en [https://portal.azure.com](https://portal.azure.com) como administrador global.

2. En el panel de navegación, seleccione **Azure Active Directory**.

3. En **Administrar**, seleccione **Usuarios**.

4. En el menú Usuarios, selecciona **Operaciones masivas > Invitación masiva**.

     ![Imagen de pantalla que muestra la página Todos los usuarios con las opciones de menú Operaciones masivas e Invitación masiva resaltadas](./media/lp1-mod3-bulk-invite-option.png)

5. En el panel de invitación masiva a usuarios, seleccione **Descargar** para obtener una plantilla CSV de ejemplo con propiedades de invitación.

6. Use un editor para ver el archivo .csv y revise la plantilla.

7. Abra la plantilla .csv y agregue una línea por cada usuario invitado. Los valores obligatorios son:

    - **Dirección de correo electrónico para enviar la invitación**: el usuario que recibirá una invitación.
    - **URL de redireccionamiento**: la dirección URL a la que se reenviará al usuario invitado después de que acepte la invitación.

    ![Imagen de pantalla que muestra el archivo CSV de la plantilla de invitación masiva de usuarios de ejemplo](./media/lp1-mod3-template-csv.png)

8. Guarde el archivo.

9. En la página Invitar usuarios en bloque, en **Cargue el archivo csv**, vaya al archivo.

     **Nota**: al seleccionar el archivo, empieza la validación del archivo .csv.

10. Cuando finalice la validación del contenido del archivo, aparecerá el mensaje **Archivo cargado correctamente**. Si hay errores, debe corregirlos para poder enviar el trabajo.

    ![Imagen de pantalla que muestra la opción de invitación masiva de usuarios con el mensaje de archivo cargado correctamente resaltado](./media/lp1-mod3-bulk-invite-users-upload-csv.png)

11. Cuando el archivo supere la validación, seleccione **Enviar** para iniciar la operación masiva de Azure que agrega las invitaciones.

12. Para ver el estado del trabajo, selecciona **Haga clic aquí para ver el estado de cada operación**. O bien, puede seleccionar **Resultados de la operación masiva** en la sección Actividad. Para obtener más información sobre cada elemento de línea de la operación masiva, seleccione los valores de las columnas **Número de elementos correctos**, **Número de errores** o **Total de solicitudes**. Si se produjeron errores, se mostrarán sus motivos.

    ![Imagen de pantalla que muestra los resultados de una operación masiva](./media/lp1-mod3-bulk-operations-results.png)

13. Cuando finalice el trabajo, verá una notificación de que la operación masiva se completó correctamente.

#### Tarea 2: invitar a usuarios invitados con PowerShell

1. Abra PowerShell como administrador.  Para ello, busca PowerShell en Windows y elige Ejecutar como administrador.  

1. Tendrás que agregar el módulo de Azure AD PowerShell, si no lo has usado antes.  Ejecuta el comando: Install-Module AzureAD.  Cuando se te solicite, selecciona "Y" para continuar.

    ``` 
    Install-Module AzureAD
    ```

1. Confirma que el módulo se ha instalado correctamente ejecutando el comando:  

    ```
    Get-Module AzureAD 
    ```

1. Después, tendrás que iniciar sesión en Azure ejecutando:  

    ```
    Connect-AzureAD
    ```
    
1. La ventana de inicio de sesión de Microsoft aparecerá para que inicies sesión en Azure AD.  

1. Para comprobar que tienes conexión y para ver los usuarios existentes, ejecuta:  

    ```
    Get-AzureADUser 
    ```

1. Ya puedes invitar a un usuario invitado.  El comando siguiente se rellenará con la información del usuario y se ejecutará.  Si tienes más de un usuario que agregar, puedes usar un archivo TXT de Bloc de notas para agregar la información del usuario y copiar o pegar en PowerShell. 

    ```
    New-AzureADMSInvitation -InvitedUserDisplayName "Display" -InvitedUserEmailAddress name@emaildomain.com -InviteRedirectURL https://myapps.microsoft.com -SendInvitationMessage $true 
    ```

Ahora sabes cómo invitar a usuarios al portal de Azure AD, al centro de administración de Microsoft 365, de forma masiva con un archivo CSV y con comandos de PowerShell.
