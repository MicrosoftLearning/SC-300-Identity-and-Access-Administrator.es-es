---
lab:
  title: "12: administrar los valores de bloqueo inteligente de Microsoft\_Entra"
  learning path: '02'
  module: Module 02 - Implement an Authentication and Access Management Solution
---

# Laboratorio 12: administrar los valores de bloqueo inteligente de Microsoft Entra

### Tipo de inicio de sesión = Administración de Microsoft 365

## Escenario del laboratorio

Debes configurar las opciones de protección de contraseña adicionales para tu organización.

#### Tiempo estimado: 5 minutos

### Ejercicio 1: administrar valores de bloqueo inteligente de Microsoft Entra

#### Tarea: agregar bloqueos inteligentes

En función de los requisitos de la organización, puede personalizar los valores de bloqueo inteligente de Microsoft Entra. Para personalizar la configuración del bloqueo inteligente con valores específicos de su organización, los usuarios necesitan una licencia de Microsoft Entra ID Premium P1 o superior.

1. Ve a [https://entra.microsoft.com](https://entra.microsoft.com) e inicia sesión con una cuenta de administrador global para el directorio.

2. Abre el menú del portal y selecciona  **Identidad**.

3. En el menú Identidad, abre el menú **Protección**.

4. En el menú de la izquierda, selecciona **Métodos de autenticación**.

5. Después selecciona **Protección de contraseña**.

    ![Imagen de pantalla que muestra la página Métodos de autenticación y las selecciones resaltadas para navegar hasta Autenticación de contraseña ](./media/lp2-mod3-browse-to-password-protection.png)

6. En la configuración de Protección de contraseña, en el cuadro **Duración del bloqueo en segundos**, establece el valor en **120**.

7. Junto a **Modo**, seleccione **Forzado**.

8. Guarda los cambios.

    **NOTA**: cuando se desencadene el umbral de bloqueo inteligente, recibirás el siguiente mensaje mientras la cuenta esté bloqueada:
    - Su cuenta se bloqueó temporalmente para impedir un uso no autorizado. Vuelva a intentarlo y, si sigue teniendo problemas, póngase en contacto con su administrador.

9. Para probarlo, elige un usuario en el inquilino de Microsoft Entra, accede a <login.microsoftonline.com> con un explorador privado y escribe una contraseña incorrecta hasta que la cuenta reciba la notificación de que está bloqueada.
