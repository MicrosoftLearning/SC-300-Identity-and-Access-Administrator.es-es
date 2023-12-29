---
lab:
  title: '15: Configurar una directiva de registro de MFA'
  learning path: '02'
  module: Module 02 - Implement an Authentication and Access Management Solution
---

# Laboratorio 15: Configurar una directiva de registro de MFA

## Escenario del laboratorio

La autenticación multifactor proporciona un medio para comprobar quién está usando más que un nombre de usuario y una contraseña. Ofrece una segunda capa de seguridad a los inicios de sesión de usuario. Para que los usuarios puedan responder a las solicitudes MFA, primero deben registrarse para la autenticación multifactor de Microsoft Entra. Debes configurar la directiva de registro de MFA de tu organización de Microsoft Entra para que se asigne a todos los usuarios.

#### Tiempo estimado: 10 minutos

### Ejercicio 1: configurar la directiva de registro de MFA

#### Tarea 1: configuración de directiva

1. Inicia sesión en [https://entra.microsoft.com]( https://entra.microsoft.com) con una cuenta de administrador global.

2. Abre el menú del portal y selecciona  **Microsoft Entra ID**.

3. En el menú de la izquierda en **Identidad**, selecciona **Protección**.

4. En la página Seguridad, en el panel de navegación izquierdo, selecciona **Protección de identidad**.

5. En la página Protección de identidades, en el panel de navegación izquierdo, en **Proteger**selecciona **Directiva de registro de MFA**.

    ![Imagen de pantalla que muestra la página de Directiva de registro de autenticación multifactor con la ruta de exploración resaltada](./media/lp2-mod4-browse-to-mfa-registration-policy.png)

6. En **Asignaciones**

7. En **Asignaciones**, seleccione **Todos los usuarios** y revise las opciones disponibles.

8. Puede elegir entre **Todos los usuarios** o **Seleccionar individuos o grupos** si quiere limitar el lanzamiento.

9. Además, puede decidir excluir usuarios de la directiva.

10. En **Controles**, observe que **Requerir registro de autenticación multifactor de Microsoft Entra ID** está seleccionado y no se puede cambiar.


#### Tarea 2: configurar la directiva de protección de identidades de Microsoft Entra para el registro de MFA

**Nota**: La protección de identidades de Microsoft Entra requiere que se active Microsoft Entra ID Premium P2. 

1. En el Centro de administración de Microsoft Entra, ve a **Microsoft Entra Identity Protection** en la barra de búsqueda.

1. En **Proteger** en el menú, selecciona **Directiva de registro de MFA**.

1. En **Asignaciones**, selecciona **Todos los usuarios** en Usuarios y selecciona un usuario para aplicar MFA.

1. Cambia **Aplicación de las directivas** de **Desactivado** a **Activado**.

1. Seleccione **Guardar**.

Esto requerirá que el usuario complete el registro de MFA la próxima vez que intentes iniciar sesión.

1. En un explorador privado, ve a `https://login.microsoftonline.com`. Introduce un nombre de usuario y una contraseña del inquilino.  Ten en cuenta los requisitos de información de seguridad adicionales que se pide al usuario que escriba.
