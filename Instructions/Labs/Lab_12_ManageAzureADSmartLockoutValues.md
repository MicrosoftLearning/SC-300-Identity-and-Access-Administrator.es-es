---
lab:
  title: 12 - Administrar los valores de bloqueo inteligente de Azure AD
  learning path: '02'
  module: Module 02 - Implement an Authentication and Access Management Solution
---

# Laboratorio 12: administración de los valores de bloqueo inteligente de Azure AD

## Escenario del laboratorio

Debes configurar las opciones de protección con contraseña adicionales para tu organización.

#### Tiempo estimado: 5 minutos

### Ejercicio 1: administración de los valores de bloqueo inteligente de Azure AD

#### Tarea: agregar bloqueos inteligentes

En función de los requisitos de su organización, puede personalizar los valores de bloqueo inteligente de Azure AD. Para personalizar la configuración del bloqueo inteligente con valores específicos de su organización, los usuarios necesitan una licencia de Azure AD Premium P1 o superior.

1. Ve a [https://portal.azure.com](https://portal.azure.com) e inicia sesión con una cuenta de administrador global para el directorio.

2. Abre el menú del portal y después, selecciona  **Azure Active Directory**.

3. En la página Azure Active Directory, en **Administrar**, selecciona **Seguridad**.

4. En la página Seguridad, en la navegación izquierda, selecciona **Métodos de autenticación**.

5. En el panel de navegación izquierdo, seleccione **Protección de contraseñas**.

    ![Imagen de pantalla que muestra la hoja Métodos de autenticación y las selecciones resaltadas para navegar hasta Autenticación de contraseña](./media/lp2-mod3-browse-to-password-protection.png)

6. En la configuración Protección de contraseña, en el cuadro **Duración del bloqueo en segundos**, establece el valor en **120**.

7. Junto a **Modo**, seleccione **Forzado**.

8. Guarde los cambios.

    **NOTA**: cuando se desencadene el umbral de bloqueo inteligente, recibirás el siguiente mensaje mientras la cuenta esté bloqueada.
    - Su cuenta se bloqueó temporalmente para impedir un uso no autorizado. Vuelva a intentarlo y, si sigue teniendo problemas, póngase en contacto con su administrador.

9. Para probarlo, elige un usuario en el inquilino de Azure AD, ve en un explorador InPrivate a <login.microsoftonline.com> y escribe una contraseña incorrecta hasta que la cuenta reciba la notificación de que está bloqueada.
