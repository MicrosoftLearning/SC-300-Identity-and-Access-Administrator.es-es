---
lab:
  title: "15 - Configurar una directiva de registro de autenticación multifactor de Azure\_AD"
  learning path: '02'
  module: Module 02 - Implement an Authentication and Access Management Solution
---

# Laboratorio 15: configurar una directiva de registro de autenticación multifactor de Azure AD

## Escenario del laboratorio

Azure AD Multi-Factor Authentication proporciona un medio para verificar tu identidad más allá del nombre de usuario y la contraseña. Ofrece una segunda capa de seguridad a los inicios de sesión de usuario. Para que los usuarios puedan responder a las solicitudes MFA, primero deben registrarse para Azure AD Multi-Factor Authentication. Debes configurar la directiva de registro de MFA de la organización de Azure AD para que se asigne a todos los usuarios.

#### Tiempo estimado: 10 minutos

### Ejercicio 1: configuración de la directiva de registro de MFA

#### Tarea 1: configurar directivas

1. Inicia sesión en [https://portal.azure.com]( https://portal.azure.com) con una cuenta de administrador global.

2. Abre el menú del portal y después, selecciona  **Azure Active Directory**.

3. En la página Azure Active Directory, en **Administrar**, selecciona **Seguridad**.

4. En la página Seguridad, en el panel de navegación izquierdo, selecciona **Protección de identidad**.

5. En la hoja Protección de identidades, en el panel de navegación izquierdo, en **Proteger**, selecciona **Directiva de registro de autenticación multifactor**.

    ![Imagen de pantalla que muestra la página de Directiva de registro de autenticación multifactor con la ruta de exploración resaltada](./media/lp2-mod4-browse-to-mfa-registration-policy.png)

6. En **Asignaciones**

7. En **Asignaciones**, seleccione **Todos los usuarios** y revise las opciones disponibles.

8. Puede elegir entre **Todos los usuarios** o **Seleccionar individuos o grupos** si quiere limitar el lanzamiento.

9. Además, puede decidir excluir usuarios de la directiva.

10. En **Controles**, observe que la opción **Require Azure AD MFA registration** (Requerir registro de Azure AD MFA) está seleccionada y no se puede cambiar.


#### Tarea 2: configurar la directiva de Azure AD Identity Protection para el registro de MFA

**Nota**: Azure AD Identity Protection requiere la activación de Azure AD Premium P2. 

1. En Azure Portal, ve a **Azure AD Identity Protection** en la barra de búsqueda.

1. En **Proteger** en el menú, selecciona **directiva de registro de MFA**.

1. En **Asignaciones**, selecciona **Todos los usuarios** en Usuarios, y selecciona un usuario para aplicarle MFA.

1. Cambia **Aplicación de directivas** de **Desactivada** a **Activada**.

1. Seleccione **Guardar**.

Esto requerirá que el usuario complete el registro de MFA la próxima vez que intente iniciar sesión.

1. En un explorador privado, ve a `https://login.microsoftonline.com`. Introduce un nombre de usuario y una contraseña del inquilino.  Observa los requisitos adicionales de información de seguridad que se le piden al usuario.
