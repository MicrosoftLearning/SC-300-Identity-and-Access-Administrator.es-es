---
lab:
  title: '18: directivas de acceso a Defender for Cloud Apps'
  learning path: '03'
  module: Module 03 - Implement Access Management for Apps
---

# 18: políticas de acceso y sesión de Defender for Cloud Apps

## Escenario del laboratorio

Microsoft Defender for Cloud Apps nos permite crear directivas de acceso condicional adicionales específicas para las aplicaciones en la nube que estamos supervisando.  La creación de estas directivas se puede realizar desde el menú Control del portal de Microsoft Defender for Cloud Apps.

#### Tiempo estimado: 20 minutos

### Ejercicio 1: creación y prueba de la directiva de control de aplicaciones de acceso condicional

#### Tarea 1: confirmar que PradeepG tiene acceso incondicional a Forms

1. Inicia una nueva ventana de **navegación de InPrivate**.
2. Conectarse a [https://forms.microsoft.com](https://forms.microsoft.com).
3. Selecciona iniciar sesión en la esquina superior derecha de la página.
4. Inicia sesión como Pradeep Gupta.
   - Nombre de usuario = PradeepG@<<<your lab hoster provided domain>>>
   - Contraseña = la contraseña de la pestaña de recursos
5. Confirma que Microsoft Forms se abre y que no recibe ningún mensaje de advertencia.
6. Cierra la ventana del explorador de InPrivate.

#### Tarea 2: configurar Azure AD para que funcione con Defender for Cloud Apps

1. Ve a [portal.azure.com](portal.azure.com) y a Azure Active Directory.

2. En **Administrar** seleccione **Seguridad**.

3. En **Proteger**, seleccione **Acceso condicional**.

4. Selecciona la lista desplegable **+ Nueva directiva** y selecciona **Crear nueva directiva**.

5. Escribe un nombre de directiva, como **Supervisar Pradeep con Forms**.

6. En **Usuarios o identidades de carga de trabajo**, selecciona **Usuarios específicos incluidos**, selecciona **Seleccionar usuarios y grupos** y marca **Usuarios y grupos**.

7. Elige la cuenta **Pradeep Gupta** para el inquilino del laboratorio y selecciona **Seleccionar**.

8. En **Aplicaciones en la nube o acciones**, seleccione **No se ha seleccionado ninguna aplicación o acción ni ningún contexto de autenticación**.

9. Selecciona **Seleccionar aplicaciones** y después elige **Microsoft Forms**, y **Seleccionar**. 

10. En **Controles de acceso**, selecciona **Sesión** y ** 0 controles seleccionados**.

11. Selecciona el cuadro **Usar control de aplicaciones de acceso condicional**, deja el valor predeterminado de **Solo supervisar** y elige **Seleccionar**.

12. En **Habilitar directiva**, selecciona **Activada** y después selecciona **Crear**.

#### Tarea 3: iniciar sesión en Forms y validar que el acceso condicional esté supervisando

1. Inicia una nueva ventana de **navegación de InPrivate**.
2. Conectarse a [https://forms.microsoft.com](https://forms.microsoft.com).
3. Selecciona iniciar sesión en la esquina superior derecha de la página.
4. Inicia sesión como Pradeep Gupta.
   - Nombre de usuario = PradeepG@<<<your lab hoster provided domain>>>
   - Contraseña = la contraseña de la pestaña de recursos
5. Confirmar que Pradeep tiene acceso y que recibe un nuevo mensaje:
   - Tu empresa supervisa el uso de esta aplicación.
6. Cierra la ventana del explorador de InPrivate.

### Ejercicio 2: configuración de alertas en Microsoft Defender for Cloud Apps

#### Tarea 1: acceder a Microsoft Defender for Cloud Apps y crear un control de aplicaciones de acceso condicional

El registro de la aplicación establece una relación de confianza entre la aplicación y la plataforma de identidad de Microsoft. La confianza es unidireccional: la aplicación confía en la plataforma de identidad de Microsoft y no al revés.

1. Inicia sesión en [https://security.microsoft.com](https://security.microsoft.com) con una cuenta de administrador global.

1. En el menú izquierdo, desplázate hasta la parte inferior y selecciona **Más recursos**.

1. En la ventana **Más recursos**, busca y selecciona **Abrir** en **Microsoft Defender for Cloud Apps**.  Esto te llevará al portal de **Microsoft Defender for Cloud Apps** en la cuenta de Microsoft 365.

1. En el menú del portal de **Microsoft Defender for Cloud Apps**, selecciona la flecha desplegable de **Control** y selecciona **Directivas**.

1. Seleccione **+ Crear directiva**. Selecciona **Directiva de acceso**.

1. Introduce un nombre para la directiva, como **Supervisar el acceso a Microsoft Forms**.

1. Puedes dejar la configuración de **Categoría** como **Control de acceso**.

1. En **Actividades que coincidan con todo lo siguiente**, selecciona el menú desplegable de **Intune compatible, Azure AD híbrido unido** y anula la selección de **Agregar Azure AD híbrido**.

1. Selecciona la lista desplegable **Seleccionar aplicaciones**.  Selecciona **Microsoft Forms**.

1. Deja **Acciones** como **Prueba**.

1. En **Alertas**, deja seleccionada **Crear una alerta...** y elige **Enviar una alerta por correo electrónico**.

1. Escribe la dirección de correo electrónico del administrador del laboratorio y selecciona **Entrar** en el teclado.

1. Selecciona **Crear** para crear la directiva.

#### Tarea 2: iniciar sesión como Pradeep en Forms para desencadenar la actividad

1. Inicia una nueva ventana de **navegación de InPrivate**.
2. Conectarse a [https://forms.microsoft.com](https://forms.microsoft.com).
3. Selecciona iniciar sesión en la esquina superior derecha de la página.
4. Inicia sesión como Pradeep Gupta.
   - Nombre de usuario = PradeepG@<<<your lab hoster provided domain>>>
   - Contraseña = la contraseña de la pestaña de recursos
5. Confirmar que Pradeep tiene acceso y que recibe un nuevo mensaje:
   - Tu empresa supervisa el uso de esta aplicación.
6. Cierra la ventana del explorador de InPrivate.

#### Tarea 3: revisar la actividad en Defender for Cloud Apps

1. Vuelve al explorador que ejecuta Defender for Cloud Apps.
2. Actualiza el explorador para asegurarte de que se descargan los datos más recientes.
3. En el menú **Investigar**, selecciona **Registro de actividad**.
4. Con **Aplicación: filtro**, selecciona **Microsoft Forms** de la lista.
5. Observa los registros de inicio de sesión de Pradeep.
