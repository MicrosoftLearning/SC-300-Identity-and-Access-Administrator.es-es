---
lab:
  title: '05: agregar usuarios invitados a un directorio'
  learning path: '01'
  module: Module 01 - Implement an identity management solution
---

# Laboratorio 05: agregar usuarios invitados al directorio

### Tipo de inicio de sesión = Administración de Microsoft 365

## Escenario del laboratorio

Tu empresa trabaja con muchos proveedores y, en ocasiones, debes agregar algunas cuentas de proveedor al directorio como invitado.

#### Tiempo estimado: 20 minutos

### Ejercicio 1: agregar usuarios invitados a un directorio

#### Tarea: agregar el usuario invitado

1. Inicia sesión en  [https://entra.Microsoft.com](https://entra.microsoft.com) como usuario que tiene asignado un rol de directorio de administrador limitado o el rol de invitador de usuarios invitados o administrador global.

2. Selecciona **Identidad**.

3. En **Usuarios**, selecciona **Todos los usuarios**.

4. Selecciona  **+ Nuevo usuario**.

5. En la página Nuevo usuario, selecciona **Invitar a usuario externo** y después agrega su información como usuario invitado.

    **NOTA**: No se admiten direcciones de correo electrónico de grupo. Escribe la dirección de correo electrónico de una persona. Además, algunos proveedores de correo electrónico permiten a los usuarios agregar un signo más (+) y texto adicional a sus direcciones de correo electrónico, ya que ello sirve de ayuda a algunas funciones como el filtrado de la bandeja de entrada. Sin embargo, Microsoft Entra ID no admite actualmente signos más (+) en las direcciones de correo electrónico. Para evitar problemas de entrega, omita el signo más y los caracteres siguientes hasta el símbolo @.

6. Escribe una dirección de correo electrónico como **sc300externaluser1@sc300email.com**.

7. Seleccione la pestaña **Propiedades**.

8. En la pantalla de Usuarios, comprueba que aparece tu cuenta y, en la columna **Tipo de usuario**, comprueba que aparece **Invitado**.

9. Cuando termines, selecciona **Revisar + invitar** y después selecciona **Invitar**.


Después de enviar la invitación, la cuenta de usuario se agrega automáticamente al directorio como invitado.


### Ejercicio 2: invita a usuarios invitados de forma masiva

#### Tarea 1: hacer una invitación masiva de usuarios

Se ha establecido una asociación reciente con otra empresa. Por ahora, los empleados de la empresa asociada se agregarán como invitados. Debes asegurarte de que puedes importar varios usuarios invitados a la vez.

1. Inicia sesión en [https://entra.microsoft.com](https://entra.microsoft.com) como administrador global.

2. En el panel de navegación, seleccione **Identidad**.

3. En **Usuarios**, selecciona **Todos los usuarios**.

4. En el menú Usuarios, selecciona **Operaciones masivas > Invitación masiva**.

   ![Imagen de pantalla que muestra la página Todos los usuarios con las opciones de menú Operaciones masivas e Invitación masiva resaltadas](./media/lp1-mod3-bulk-invite-option.png)

5. En el panel de invitación masiva a usuarios, seleccione **Descargar** para obtener una plantilla CSV de ejemplo con propiedades de invitación.

6. Use un editor para ver el archivo .csv y revise la plantilla.

7. Abra la plantilla .csv y agregue una línea por cada usuario invitado. Los valores obligatorios son:

    - **Dirección de correo electrónico para enviar la invitación**: el usuario que recibirá una invitación.
    - **URL de redireccionamiento**: la dirección URL a la que se reenviará al usuario invitado después de que acepte la invitación.

    ![Imagen de pantalla que muestra el archivo .csv de la plantilla de invitación masiva de usuarios de ejemplo.](./media/lp1-mod3-template-csv.png)

**Sugerencia de laboratorio**: los usuarios que aparecen en la captura de pantalla y los archivos de plantilla son ejemplos, no existen realmente.  Deberás agregar usuarios reales para probar completamente esta característica.

8. Guarde el archivo.

9. En la página Invitar usuarios en bloque, en **Cargue el archivo csv**, vaya al archivo.

     **Nota**: Al seleccionar el archivo, empieza la validación del archivo .csv.

10. Cuando finalice la validación del contenido del archivo, aparecerá el mensaje **Archivo cargado correctamente**. Si hay errores, debe corregirlos para poder enviar el trabajo.

    ![Imagen de pantalla que muestra la opción de invitación masiva de usuarios con el mensaje de archivo cargado correctamente resaltado](./media/lp1-mod3-bulk-invite-users-upload-csv.png)

11. Cuando el archivo supere la validación, seleccione **Enviar** para iniciar la operación masiva de Azure que agrega las invitaciones.

12. Para ver el estado del trabajo, selecciona **Haz clic aquí para ver el estado de cada operación**. O bien, puede seleccionar **Resultados de la operación masiva** en la sección Actividad. Para obtener más información sobre cada elemento de línea de la operación masiva, seleccione los valores de las columnas **Número de elementos correctos**, **Número de errores** o **Total de solicitudes**. Si se produjeron errores, se mostrarán sus motivos.

    ![Imagen de pantalla que muestra los resultados de una operación masiva.](./media/lp1-mod3-bulk-operations-results.png)

13. Cuando finalice el trabajo, verá una notificación de que la operación masiva se completó correctamente.

#### Tarea 2: invitar a usuarios invitados con PowerShell

1. Abra PowerShell como administrador. Para ello, busca PowerShell en Windows y elige Ejecutar como administrador. 

**Nota:** Debes tener PowerShell versión 7.2 o posterior para que este laboratorio funcione.  Cuando se abra PowerShell, aparecerá una versión en la parte superior de la pantalla. Si estás ejecutando una versión anterior, actualízala o se producirá un error en esta parte del laboratorio.

**Sugerencia de laboratorio**: la característica TouchType del entorno de laboratorio tiene problemas al escribir en PowerShell. Si inicias el Bloc de notas en el laboratorio, usa TouchType para cargar las instrucciones de PowerShell en el Bloc de notas. Puedes usar **Cortar y pegar** para introducirlas en PowerShell sin escribir.

2. Deberás instalar el módulo de PowerShell Microsoft.Graph si no lo has usado antes.  Ejecuta los dos comandos siguientes y cuando se te pida confirmación pulsa Y:

    ```
    Install-Module Microsoft.Graph
    ```
3. Confirma que el módulo Microsoft.Graph está instalado:

    ```
    Get-InstalledModule Microsoft.Graph
    ```
    

4. Después deberás iniciar sesión en Azure mediante la ejecución de:  

    ```
    Connect-MgGraph -Scopes "User.ReadWrite.All"
    ``` 
    Se abrirá el explorador Edge y se te pedirá que inicies sesión.  Usa la cuenta de administrador MOD para conectarte.  Marca el cuadro de consentimiento, acepta la solicitud de permisos y cierra la ventana del explorador.

5. Establece los valores del correo electrónico y redirecciona al usuario externo:

    ```
    Import-Module Microsoft.Graph.Identity.SignIns
    
    $params = @{
        invitedUserEmailAddress = "admin@fabrikam.com"
        inviteRedirectUrl = "https://myapp.contoso.com"
    }
    ```

6. Se envió el comando MgInvitation para invitar al usuario externo:

    ```
    New-MgInvitation -BodyParameter $params
    ```

7. Puedes cerrar PowerShell en este momento.
    
Ahora sabes cómo invitar a usuarios en el Centro de administración de Microsoft Entra y el Centro de administración de Microsoft 365, enviar invitaciones masivas con un archivo CSV e invitar a usuarios con comandos de PowerShell.  Puedes ir al Centro de administración de Microsoft Entra y comprobar todos los usuarios para ver que administrador se ha agregado como usuario externo.
