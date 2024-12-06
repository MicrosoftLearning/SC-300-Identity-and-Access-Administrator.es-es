---
lab:
  title: '09: habilitar el restablecimiento de contraseña de autoservicio de Microsoft Entra'
  learning path: '02'
  module: Module 02 - Implement an Authentication and Access Management Solution
---

# Laboratorio 09: configurar e implementar el autoservicio de restablecimiento de contraseña

### Tipo de inicio de sesión = Administración de Microsoft 365

## Escenario del laboratorio

La empresa ha decidido capacitar a los empleados y habilitar el autoservicio de restablecimiento de contraseña. Debes configurar este ajuste en tu organización.

#### Tiempo estimado: 15 minutos

### Ejercicio 1: crear un grupo con SSPR habilitado y agregarle usuarios

#### Tarea 1: crear un grupo para asignar SSPR a

Queremos implementar SSPR en un conjunto limitado de usuarios en primer lugar para asegurarnos de que la configuración de SSPR funciona según lo previsto. Vamos a crear un grupo de seguridad para el lanzamiento limitado y agregar un usuario al grupo.

1. En el Centro de administración Microsoft Entra, abre el menú de navegación **Identidad** de la izquierda.
1. En **Grupos**, selecciona **Todos los grupos** y selecciona **Nuevo grupo** en la ventana derecha.

2. Cree un nuevo grupo con la siguiente información:

    | **Configuración**| **Valor**|
    | :--- | :--- |
    | Tipo de grupo| Seguridad|
    | Nombre del grupo| SSPRTesters|
    | Descripción del grupo| Evaluadores de implementación de SSPR|
    | Tipo de pertenencia| Asignada|
    | Miembros| Alex Wilber |
    | |  Allan Deyoung |
    | | Bianca Pisani |
  
    
3. Seleccione  **Crear**.

    ![Imagen de pantalla que muestra la página del nuevo grupo con tipo y nombre de grupo y crear resaltados](./media/lp2-mod2-create-sspr-security-group.png)

#### Tarea 2: habilitar SSPR para el grupo de prueba

Habilite SSPR para el grupo.

1. Vuelve al menú de navegación **Identidad**.

2. En  **Protección**, selecciona  **Restablecimiento de contraseña**.

3. En la página Propiedades del panel de restablecimiento de contraseña, en **Restablecimiento de contraseña de autoservicio habilitado**, selecciona  **Seleccionado**.

4. En **Seleccionar grupo**, reemplaza el SSPRSecurityGroupUsers existente con **SSPRTesters** que acabas de crear.

5. En la página Propiedades de la página de restablecimiento de contraseña, selecciona  **Guardar**.

    ![Imagen de pantalla que muestra la página Propiedades de restablecimiento de contraseña con seleccionada, seleccionar grupo y guardar resaltado](./media/lp2-mod2-enable-password-reset-for-selected-group.png)

6. En la pantalla **Restablecimiento de contraseña**, busca en  **Administrar*, selecciona y revisa los valores predeterminados de cada uno de los **métodos de autenticación **, ** Registro**, **Notificaciones** y **Configuración de personalización**.

    **Nota** Es importante tener **el teléfono** seleccionado como uno de los métodos de autenticación para el resto de este laboratorio, pero también puedes tener otras opciones.

#### Taks 3: registrarse en SSPR con Allan

Ahora que ya tenemos completada la configuración de SSPR, podemos registrar un número de teléfono móvil para el usuario que hemos creado.

1. Abra otro explorador o abra una sesión de explorador InPrivate o incógnito y, a continuación, vaya a [https://aka.ms/ssprsetup](https://aka.ms/ssprsetup).

    Esto se hace para asegurarse de que se te pida la autenticación del usuario.

2. Inicia sesión como **AllanD@**`<<organization-domain-name>>.onmicrosoft.com` con la contraseña proporcionada.

    **Nota**: Reemplaza nombre-dominio-organización por tu nombre de dominio.

3. Cuando se te pida que actualices la contraseña, escribe una contraseña nueva de tu elección. Asegúrese de registrar la contraseña nueva.

4. Si aparece un mensaje para preguntarte si quieres mantener iniciada la sesión, elige Sí.

5. En el cuadro de diálogo **Más información requerida**, seleccione **Siguiente**.

6. En la página Mantener la cuenta segura, selecciona **Siguiente** para usar la aplicación Authenticator.

7. Sigue las instrucciones de la pantalla para configurar tu cuenta de Authenticator al escanear el código QR.

8. Para completar el proceso, selecciona **Listo** cuando te hayas registrado correctamente.

  - **Nota:** en este momento te has registrado para SSPR y MFA en un solo paso.

11. Cierre el explorador. No es necesario completar el proceso de inicio de sesión.

#### Tarea 2: prueba SSPR

Ahora vamos a comprobar si el usuario puede restablecer su contraseña.

1. Abra otro explorador o abra una sesión de explorador InPrivate o incógnito y, a continuación, vaya a  [https://portal.azure.com](https://portal.azure.com).

    Esto se hace para asegurarse de que se le pida la autenticación del usuario.

2. Escribe **AlexW@**`<<organization-domain-name>>.onmicrosoft.com` y después selecciona **Siguiente**.

    **Nota**: Reemplaza nombre-dominio-organización por tu nombre de dominio.

3. En la pantalla Escribir contraseña, seleccione **Olvidé mi contraseña**.

4. En la página Volver a la cuenta, complete la información solicitada y, después, seleccione **Siguiente**.

5. Sigue las instrucciones en pantalla para obtener el código de verificación de la aplicación Microsoft Authenticator.

6. Escriba el código de verificación y, a continuación, seleccione **Siguiente**.

7. En el paso elija una nueva contraseña, escriba y confirme la nueva contraseña.

8. Cuando termine, seleccione **Finalizar**.

9. Inicia sesión como **AllanD** con la nueva contraseña que has creado.

10. Escriba el código de verificación y, a continuación, compruebe que puede completar el proceso de inicio de sesión.

11. Cuando termine, cierre el explorador.

#### Tarea 5: ¿qué ocurre si lo intentas con un usuario que no esté en el grupo SSPRTesters?

1. Como prueba, abre una nueva ventana del explorador InPrivate e intenta iniciar sesión en Azure Portal como GradyA y selecciona la opción **He olvidado mi contraseña**.
