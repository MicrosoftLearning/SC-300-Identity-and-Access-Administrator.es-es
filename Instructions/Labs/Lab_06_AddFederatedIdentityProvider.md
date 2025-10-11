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

4. En la página Nuevo proyecto, asigna un nombre al proyecto: +++MyB2BApp+++ y después selecciona **Crear**.

5. Abra el proyecto nuevo seleccionando el vínculo en el cuadro de mensaje Notificaciones o con el menú del proyecto en la parte superior de la página.

6. En el menú de la izquierda, selecciona **API y servicios** y después selecciona **Pantalla de consentimiento de OAuth**.

7. Seleccione el botón **Introducción**.

8. En la pantalla Información de la aplicación, escribe la siguiente información:

| Sección | Nombre del campo | Valor |
| :---    | :---    | :---  |
| 1 Información de la aplicación: | | |
|            | Nombre de la aplicación | +++Microsoft Entra ID+++ |
|            | Correo electrónico de soporte técnico del usuario | Selecciona el nombre en el elemento desplegable |
| 2 Público | | |
|            | Interna/Externa | **Externo** |
| 3 Información de contacto | | |
|            | Direcciones de correo | Usa la misma dirección de correo electrónico que antes |
| 4 Finalizar | | |
|            | Contrato | Marca la casilla |

9. Selecciona el botón **Crear** para continuar.

10. Selecciona el botón **Crear cliente OAuth**.

11. Elige **Tipo de aplicación = Aplicación web**.

12. Acepta el nombre predeterminado de la aplicación.

13. En los **orígenes de JavaScript autorizados**, selecciona el botón **+ Agregar URI**.

14. Escribe el URI +++https://microsoftonline.com+++ para el valor.

15. En **URI de redirección autorizados**, selecciona el botón **+ Agregar un URI**.  Tendrás que agregar tres URI diferentes en esta sección:

 - **Primer URI** = +++https://login.microsoftonline.com+++
 - **Segundo URI** = +++https://login.microsoftonline.com/te/**tenant ID**/oauth2/authresp+++ (donde  <tenant ID> es el identificador del inquilino)
 - **Tercer URI** = +++https://login.microsoftonline.com/te/**tenant name**.onmicrosoft.com/oauth2/authresp+++ (donde <tenant name> es el nombre del inquilino)

**Sugerencia de laboratorio**: este paso te puede resultar más fácil si usas el Bloc de notas en la máquina virtual del laboratorio para crear estos URI y, a continuación, copias y pegas desde allí.

**Sugerencia de laboratorio 2**: los resultados deben tener un aspecto similar al siguiente, con el identificador y el nombre del inquilino.

| URI # | Vínculo |
| :--- | :--- |
| URI 1 | https://login.microsoftonline.com |
| URI 2 | https://login.microsoftonline.com/te/aaaa1111bbbb2222cccc/oauth2/authresp |
| URI 3 | https://login.microsoftonline.com/te/MyTenantName.onmicrosoft.com/oauth2/authresp |

16. Seleccione el botón **Crear**.

17. Cuando se crea el elemento, copia el **Id. de cliente** y el **secreto de cliente** en el Bloc de notas para el usuario más adelante.

18. Puedes dejar el proyecto en este estado, no es necesario publicarlo.

#### Tarea 2: agregar un usuario de prueba

1. En el menú de la izquierda, selecciona el elemento **Público**.

2. En la sección ***Usuarios de la prueba** de la página, selecciona **+ Agregar usuarios**.

3. Escribe la cuenta de gmail que usas para este laboratorio.

4. Seleccione **Guardar**.

#### Tarea 3: Agregar un dominio autorizado a la personalización de marca

1. En el menú de la izquierda, selecciona el elemento **Personalización de marca**.

2. Desplázate hasta la parte inferior de la página.

3. En la sección **Dominios autorizados**, agrega el dominio **microsoftonline.com**.

4. En **información de contacto del desarrollador**, agrega la dirección de correo electrónico que usas para este laboratorio.

5. Seleccione **Guardar**.


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
1. Si has usado una cuenta de Gmail existente, no olvides eliminar la cuenta con **External Identities | Todos los proveedores de identidades**. También puedes volver a la consola del desarrollador de Google y eliminar el proyecto que has creado.

2. Abra Microsoft Entra ID.

3. Ve a Usuarios y selecciona **Todos los usuarios**.

4. Selecciona **+ Nuevo usuario**.

5. Selecciona **Invitar usuario externo** en el menú desplegable.

6. Introduce la información de la cuenta de Gmail que has configurado como usuario de prueba para Google App en la Tarea 2 del Ejercicio 1.

7. Escribe un mensaje personal.

8. Selecciona **Revisar e invitar** y, después, selecciona **Invitar**.


| **Nota de seguridad** |
| ----: |
| Si usas una cuenta de Gmail existente que tenga habilitadas las llaves de acceso, no podrás completar los procesos de inicio de sesión dentro del entorno de laboratorio.  La llave de acceso requiere BlueTooth, que no se puede habilitar a través de la máquina virtual.  Todavía puedes completar el laboratorio, solo tienes que realizar estas últimas tareas en un navegador InPrivate que se ejecuta fuera del entorno de laboratorio. |


#### Tarea 3: aceptar la invitación e iniciar sesión

1. Usa un explorador InPrivate para iniciar sesión en tu cuenta de gmail.

2. Abre la **Invitación de Microsoft en nombre de** en la bandeja de entrada.

3. Selecciona el vínculo **Aceptar invitación** en el mensaje.

3. Escribe el nombre de usuario y la contraseña cuando se te solicite en el cuadro de diálogo de inicio de sesión (si se te solicita).

   **NOTA** Si la federación funciona correctamente, aquí es donde verás los primeros resultados de tu nuevo proveedor de identidades externas de Google.  Irás a la pantalla de inicio de sesión y podrás iniciar sesión con tus credenciales de gmail.  Si la federación no funciona, o no se ha configurado, se enviará al usuario un correo electrónico de VERIFICACIÓN DE LA CUENTA después del inicio de sesión, para confirmar la cuenta.  Con la federación, no se necesita ninguna comprobación adicional.

   **NOTA** Si recibes un error de acceso 500, espera unos 30 segundos y actualiza la página.  Elige VOLVER A ENVIAR.  Este error es una incidencia de tiempo que se da solo en el entorno del laboratorio.

4. Lee el nuevo mensaje **permisos solicitados por:** que recibes.  Este mensaje proviene de tu dominio del laboratorio de Azure.

5. Elige **Aceptar**.

6. Una vez completado el inicio de sesión, se te enviará a MyApplications.

#### Tarea 4: iniciar sesión en Microsoft 365 con tu cuenta de Google

1. Una vez que hayas terminado el proceso de invitación de usuario externo de la tarea 3, puedes iniciar sesión directamente en Microsoft Online.

2. Abre una nueva pestaña en el explorador que tienes abierto.

   **NOTA** Si no has abierto un nuevo explorador InPrivate en la tarea 3, debes hacerlo para este paso.

3. Escribe la siguiente dirección web:

   ```
   login.microsoftonline.com
   ```

4. Selecciona **Opciones de inicio de sesión** en el cuadro de diálogo.
 
5. Selecciona **Iniciar sesión en una organización**.

6. Escribe el **nombre de dominio del inquilino de laboratorio** en el cuadro y selecciona **Siguiente**.

7. Escribe la dirección de correo electrónico de **Google** y la contraseña que has creado.

En este momento, deberías ver que tu cuenta pasa a Google para su confirmación; después entra en el portal de Microsoft Office.
