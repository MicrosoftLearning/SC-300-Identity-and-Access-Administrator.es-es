---
lab:
  title: '6: agregar un proveedor de identidades federado'
  learning path: '01'
  module: Module 01 - Implement an identity management solution
---

# Laboratorio 06: agregar un proveedor de identidades federado

### Tipo de inicio de sesión = Administración de Microsoft 365

## Escenario del laboratorio

Tu empresa trabaja con muchos proveedores y, en ocasiones, necesitas agregar algunas cuentas de proveedor al directorio como invitado y permitirles usar tu cuenta de Google para iniciar sesión.

#### Tiempo estimado: 25 minutos

### Ejercicio 1: configurar proveedores de identidades

#### Tarea 1: configurar Google como proveedor de identidades

**Nota importante:** para este ejercicio, necesitarás una cuenta de Gmail en Google. Crea una **nueva cuenta de Google** y sigue los pasos del ejercicio.  Asegúrate de anotar la dirección de correo electrónico y la contraseña, que son necesarias para completar el laboratorio.

1. Vaya a las API de Google de https://console.developers.google.com e inicie sesión con su cuenta de Google. Se recomienda utilizar una cuenta de Google compartida con el equipo.

2. Si se le solicita, acepte los términos del servicio.

**Crear un nuevo proyecto:**
3. En la parte superior de la página, selecciona el menú de proyectos para abrir la página Seleccionar un proyecto. Elija **Nuevo proyecto**.  Deja los campos restantes con la configuración predeterminada.

4. En la página Nuevo proyecto, asigna un nombre al proyecto (por ejemplo, **MyB2BApp**) y después selecciona **Crear**.

5. Abra el proyecto nuevo seleccionando el vínculo en el cuadro de mensaje Notificaciones o con el menú del proyecto en la parte superior de la página.

6. En el menú de la izquierda, selecciona **API y servicios** y después selecciona **Pantalla de consentimiento de OAuth**.

7. En Tipo de usuario, selecciona **Externo** y después **Crear**.

8. En la **pantalla de consentimiento de OAuth**, en Información de la aplicación, introduce un nombre de aplicación, como **Microsoft Entra ID**.

9. En Correo electrónico de soporte del usuario, seleccione una dirección de correo electrónico. Esto debe incluir la dirección de correo electrónico que usaste para iniciar sesión en Google.

10. En Dominios autorizados, selecciona **+ Agregar dominio** y después agrega el dominio microsoftonline.com.

   ```
   microsoftonline.com
   ```

11. En Información de contacto del desarrollador, escribe la dirección de correo electrónico de la cuenta de laboratorio que usaste para iniciar sesión en el portal.

12. Selecciona **Guardar y continuar**.

13. En el menú de la izquierda, seleccione **Credenciales**.

14. Selecciona **+ Crear credenciales** y después selecciona **Id. de cliente de OAuth**.

15. En el menú Tipo de aplicación, seleccione Aplicación web. Asigne a la aplicación un nombre adecuado, como Microsoft Entra B2B. En **URI de redireccionamiento autorizados**, agregue estos URI:

   ```
      https://login.microsoftonline.com
   ```
      https://login.microsoftonline.com/te/**tenant ID**/oauth2/authresp (donde <tenant ID> es el identificador del inquilino)
   ```
      https://login.microsoftonline.com/te/**tenant name**.onmicrosoft.com/oauth2/authresp
       (where <tenant name> is your tenant name)
   ```

**Sugerencia de laboratorio**: los resultados deben tener un aspecto similar al siguiente, con el identificador y el nombre del inquilino.
| N.º URI | Vínculo |
| :--- | :--- |
| URI 1 | https://login.microsoftonline.com |
| URI 2 | https://login.microsoftonline.com/te/aaaa1111bbbb2222cccc |
| URI 3 | https://login.microsoftonline.com/te/MyTenantName.onmicrosoft.com/oauth |

16. Seleccione **Crear**. Copia tu **Id. de cliente** y **secreto de cliente**. Los usará cuando agregue el proveedor de identidades en Azure Portal.

17. Puedes dejar el proyecto en un estado de publicación de Pruebas.

#### Tarea 2: agregar un usuario de prueba
18. Selecciona la **pantalla de consentimiento OAuth** en el menú de API y servicios.

19. En la sección **Usuarios de la prueba* de la página, selecciona **+ Agregar usuarios**.

20. Escribe la cuenta de gmail que has creado (o estás usando) para este laboratorio.

21. Seleccione **Guardar**.


### Ejercicio 2: configura Azure para trabajar con un proveedor de identidades externo

#### Tarea 1: configura la federación de Google en Microsoft Entra ID
1. Inicia sesión en [https://entra.microsoft.com](https://entra.microsoft.com) como administrador.

2. Selecciona **Microsoft Entra ID**.

3. En **Identidad**, selecciona **Identidades externas**.

4. Elige **Todos los proveedores de identidades** en el menú de la izquierda.

5. Microsoft proporciona una federación directa para **Google** como proveedor de identidades.Para iniciarlo, selecciona **+ Google** en la página **Identidades externas | Todos los proveedores de identidades**
 
6. Después de seleccionar + Google, se abrirá otra página con información adicional necesaria para configurar Google como proveedor de identidades.  

7. Escribe el **ID de cliente** y el **Secreto de cliente** que has obtenido anteriormente.

8. Seleccione **Guardar**.

Esto completa la configuración de Google como proveedor de identidades.

#### Tarea 2: invitar a tu cuenta de usuario de prueba
9. Si has usado una cuenta de Gmail existente, no olvides eliminar la cuenta con **External Identities | Todos los proveedores de identidades**. También puedes volver a la consola del desarrollador de Google y eliminar el proyecto que has creado.

10. Abra Microsoft Entra ID.

11. Ve a Usuarios y selecciona **Todos los usuarios**.

12. Selecciona **+ Nuevo usuario**.

13. Selecciona **Invitar usuario externo** en el menú desplegable.

14. Introduce la información de la cuenta de Gmail que has configurado como usuario de prueba para Google App en la Tarea 2 del Ejercicio 1.

15. Escribe un mensaje personal.

16. Seleccione **Invitar**.

#### Tarea 3: aceptar la invitación e iniciar sesión
17. Usa un explorador InPrivate para iniciar sesión en tu cuenta de gmail.

18. Abre la **Invitación de Microsoft en nombre de** en la bandeja de entrada.

19. Selecciona el vínculo **Aceptar invitación** en el mensaje.

20. Escribe el nombre de usuario y la contraseña cuando se te solicite en el cuadro de diálogo de inicio de sesión (si se te solicita).
   **NOTA** Si la federación funciona correctamente, aquí es donde verás los primeros resultados de tu nuevo proveedor de identidades externas de Google.  Irás a la pantalla de inicio de sesión y podrás iniciar sesión con tus credenciales de gmail.  Si la federación no funciona, o no se ha configurado, se enviará al usuario un correo electrónico de VERIFICACIÓN DE LA CUENTA después del inicio de sesión, para confirmar la cuenta.  Con la federación, no se necesita ninguna comprobación adicional.

   **NOTA** Si recibes un error de acceso 500, espera unos 30 segundos y actualiza la página.  Elige VOLVER A ENVIAR.  Este error es una incidencia de tiempo que se da solo en el entorno del laboratorio.

21. Lee el nuevo mensaje **permisos solicitados por:** que recibes.  Este mensaje proviene de tu dominio del laboratorio de Azure.

22. Elige **Aceptar**.

23. Una vez completado el inicio de sesión, se te enviará a MyApplications.

#### Tarea 4: iniciar sesión en Microsoft 365 con tu cuenta de Google
24. Una vez que hayas terminado el proceso de invitación de usuario externo de la tarea 3, puedes iniciar sesión directamente en Microsoft Online.

25. Abre una nueva pestaña en el explorador que tienes abierto.
   **NOTA** Si no has abierto un nuevo explorador InPrivate en la tarea 3, debes hacerlo para este paso.

26. Escribe la siguiente dirección web:

   ```
   login.microsoftonline.com
   ```

27. Selecciona **Opciones de inicio de sesión** en el cuadro de diálogo.
 
28. Selecciona **Iniciar sesión en una organización**.

29. Escribe el **nombre de dominio del inquilino de laboratorio** en el cuadro y selecciona **Siguiente**.

30. Escribe la dirección de correo electrónico de **Google** y la contraseña que has creado.
En este momento, deberías ver que tu cuenta pasa a Google para su confirmación; después entra en el portal de Microsoft Office.
