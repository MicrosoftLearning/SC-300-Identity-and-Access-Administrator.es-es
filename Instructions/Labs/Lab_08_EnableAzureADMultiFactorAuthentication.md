---
lab:
  title: '08: habilitar la autenticación multifactor'
  learning path: '02'
  module: Module 02 - Implement an Authentication and Access Management Solution
---

# Laboratorio 08: habilitar la autenticación multifactor

### Tipo de inicio de sesión = Administración de Microsoft 365

## Escenario del laboratorio

Para mejorar la seguridad de tu organización, se te ha indicado que habilites la autenticación multifactor para Microsoft Entra ID.

#### Tiempo estimado: 15 minutos

**IMPORTANTE**: se requiere una licencia de Microsoft Entra ID Premium para este ejercicio.

### Ejercicio 1: revisar y habilitar la autenticación multifactor en Azure

#### Tarea 1: revisar las opciones de autenticación multifactor de Azure

1. Ve a [https://entra.microsoft.com](https://entra.microsoft.com) e inicia sesión con una cuenta de administrador global para el directorio.

2. Usa la característica de búsqueda y busca **multifactor**.

3. En los resultados de búsqueda, selecciona **Autenticación multifactor**.

    Como alternativa, abre **Identidad**, luego selecciona **Protección** y selecciona **Autenticación multifactor**.

4. En la página Introducción, en **Configurar**, selecciona **Configuración adicional de MFA basado en la nube**.

    ![Captura de pantalla en la que se muestran las opciones de MFA en el panel](./media/lp2-mod1-set-additional-mfa-settings.png)

5. En la nueva página del explorador, puedes ver las opciones de MFA para los usuarios de Azure y la configuración del servicio.

    ![Captura de pantalla en la que se muestra la configuración de MFA](./media/lp2-mod1-mfa-settings.png)

    Aquí es donde se seleccionan los métodos de autenticación admitidos, todos ellos seleccionados en la pantalla anterior.

    También puede habilitar o deshabilitar aquí las contraseñas de aplicación, que permiten a los usuarios crear contraseñas de cuenta únicas para las aplicaciones que no admiten la autenticación multifactor. Esta característica permite que el usuario se autentique con su identidad de Microsoft Entra mediante otra contraseña específica para esa aplicación.

#### Tarea 2: configurar reglas de acceso condicional para MFA para Delia Dennis

A continuación se verá cómo configurar las reglas de directivas de acceso condicional que aplicarán MFA para los usuarios invitados que accedan a aplicaciones específicas en la red.

1. Vuelve al Centro de administración de Microsoft Entra, selecciona **Identidad**, después **Protección** y a continuación **Acceso condicional**.

2. En el menú, selecciona **+ Nueva directiva**. En el menú desplegable selecciona **+ Crear nueva directiva**.

    ![Captura de pantalla en la que se resalta el botón Nueva directiva en el Centro de administración de Microsoft Entra.](./media/lp2-mod1-azure-ad-conditional-access-policy.png)

3. Asigna un nombre a la directiva, por ejemplo, **MFA_for_Delia**.

4. Selecciona **Usuarios o identidades de cargas de trabajo** en Asignaciones.

    - Selecciona **0 usuarios o identidades de carga de trabajo seleccionadas**  
    - En la pantalla de la derecha, activa la casilla **Seleccionar usuarios y grupos** para configurar.
    - Comprueba **Usuarios y grupos** (los usuarios disponibles se rellenarán a la derecha)
    - Elige **Delia Dennis** en la lista de usuarios y después elige el botón **Seleccionar**.

5. En Recursos de destino, selecciona **No se ha seleccionado ningún recurso de destino**.

   - En la lista desplegable, asegúrate de que **las aplicaciones en la nube** estén seleccionadas.
   - En Incluir, marca **Todas las aplicaciones en la nube** y ten en cuenta la advertencia sobre el bloqueo. 
   - Ahora, en la sección Seleccionar, elige el elemento **Ninguno**.
   - En el cuadro de diálogo recién abierto, elige **Office 365**.
      - **Recordatorio:** en un laboratorio anterior le dimos a Delia Dennis una licencia de Office 365 e iniciamos sesión para asegurarnos de que funcionaba.
   - Elige **Seleccionar**.

6. Revise la sección Condiciones.

   - Elige **Sí** en el control deslizante de configuración.
   - Selecciona **Cualquier red o ubicación**.

7. En **Controles de acceso**, busca la sección **Conceder** y selecciona **0 controles seleccionados**.

8. Activa la casilla **Requerir autenticación multifactor** para aplicar MFA.

9. Asegúrate de que la opción **Requerir todos los controles seleccionados** esté seleccionada.

10. Elija **Seleccionar**.

11. Establezca **Habilitar directiva** en **Activado**.

12. Selecciona **Crear** para crear la directiva.

    ![Captura de pantalla en la que se muestra el cuadro de diálogo Agregar directiva completo](./media/lp2-mod1-conditional-access-new-policy-complete.png)

    Ahora la MFA está habilitada para las aplicaciones y el usuario seleccionados. La próxima vez que un invitado intente iniciar sesión en esa aplicación, se le pedirá que se registre para MFA.

#### Tarea 3: probar el inicio de sesión de Delia

1. Abre una nueva ventana de exploración de InPrivate.
2. Debes conectarte a https://www.office.com.
3. Selecciona la opción de Iniciar sesión.
4. Escribe **DeliaD@**`<<your domain address>>`.
5. Escribe la contraseña = Escribe la contraseña de administrador global del inquilino (Nota: consulta la pestaña "Recursos de laboratorio" para recuperar la contraseña de administrador).

**Nota**: en este punto, puede suceder una de estas dos cosas.  Deberías recibir un mensaje que te pide que configures la aplicación Authenticator y registrarte para MFA.  Sigue las indicaciones para completar el uso de tu teléfono personal.  NOTA: es posible que recibas un mensaje de error de inicio de sesión con varias opciones sobre cómo continuar.  Selecciona la opción **Intentar de nuevo**.

Puedes ver que debido a la regla de acceso condicional que creamos para Delia, se requiere MFA para iniciar la página principal de Office 365.

### Ejercicio 2: configurar MFA para que sea necesario para el inicio de sesión

#### Tarea 1: configurar MFA por usuario de Microsoft Entra

Por último, se describirá cómo configurar MFA para cuentas de usuario. Es otra manera de acceder a la configuración de la autenticación multifactor.

1. Vuelve al Centro de administración de Microsoft Entra y busca el menú de navegación izquierdo Identidad.

2. Selecciona **Usuarios** y después selecciona **Todos los usuarios**.

3. En la parte superior del panel Usuarios, selecciona **MFA por usuario**.
  - NOTA: Es posible que tengas que usar los puntos suspensivos (...) para ir al elemento de menú MFA por usuario.

   ![Captura de pantalla en la que se muestra la opción MFA](./media/lp2-mod1-users-mfa.png)

4. Se abrirá una nueva pestaña o ventana del explorador con un cuadro de diálogo de configuración de usuario de autenticación multifactor.

   Puede habilitar o deshabilitar MFA por usuario si selecciona uno y después sigue los pasos rápidos del lado derecho.

   ![Captura de pantalla en la que se muestran las opciones de MFA](./media/lp2-mod1-mfa-service-settings-and-users.png)

5. Selecciona **Adele Vance** con una marca de verificación.
6. Selecciona la opción **Habilitar MFA** en pasos rápidos.
7. Lee el elemento emergente de notificación si aparece y luego selecciona el botón **Habilitar autenticación multifactor**.
8. Selecciona **Cerrar**.
9. Observa que Adele ahora tiene **Habilitado** su estado de MFA.
10. Puedes seleccionar **configuración del servicio** para ver la pantalla de configuración de MFA, que se ha visto anteriormente en el laboratorio.
11. Cierra la pestaña Configuración de MFA.

#### Tarea 2: probar a iniciar sesión como Adele

1. Si deseas ver otro ejemplo del proceso de inicio de sesión de MFA, puedes intentar iniciar sesión como Adele.
