---
lab:
  title: '18: directivas de acceso a Defender for Cloud Apps'
  learning path: '03'
  module: Module 03 - Implement Access Management for Apps
---

# 18: directivas de acceso y sesión de Defender for Cloud Apps

### Tipo de inicio de sesión = Administración de Microsoft 365

## Escenario del laboratorio

Microsoft Defender for Cloud Apps nos permite crear directivas de acceso condicional adicionales específicas de las aplicaciones en la nube que estamos supervisando.  La creación de estas directivas se puede realizar desde el menú Control del portal de Microsoft Defender for Cloud Apps.

#### Tiempo estimado: 20 minutos

### Ejercicio 1: crear y probar la directiva de control de aplicaciones de acceso condicional

#### Tarea 1: confirmar que PradeepG tiene acceso incondicional a FORMS

1. Inicia una nueva ventana de **exploración de InPrivate**.
2. Conéctese a [https://forms.microsoft.com](https://forms.microsoft.com).
3. Selecciona inicio de sesión en la esquina superior derecha de la página.
4. Inicia sesión como Pradeep Gupta.
   - Nombre de usuario: PradeepG@<<<your lab hoster provided domain>>>
   - Contraseña: la contraseña de la pestaña de recursos
5. Confirma que Microsoft Forms se abre y que no recibes ningún mensaje de advertencia.
6. Cierra la ventana de exploración de InPrivate.

#### Tarea 2: configurar Microsoft Entra ID para que funcione con Defender for Cloud Apps

1. Navega a [https://entra.microsoft.com](https://entra.microsoft.com) y ve a Microsoft Entra ID.

2. En **Identidad**, selecciona **Protección**.

3. Selecciona después **Acceso condicional**.

4. Seleccione **+ Crear nueva directiva**.

5. Escribe un nombre de directiva, como **Supervisar el uso de Forms de Pradeep**.

6. En **Asignaciones**, elige **0 usuarios y grupos seleccionados**, selecciona **Usuarios específicos incluidos** y después **Seleccionar usuarios y grupos** y marca **Usuarios y grupos**.

7. Elige la cuenta de **Pradeep Gupta** para el inquilino del laboratorio y selecciona **Seleccionar**.

8. En **Recursos de destino**, seleccione **No se ha seleccionado ningún recurso de destino**.

9. Selecciona **Seleccionar aplicaciones** y luego elige **Microsoft Forms** y selecciona **Seleccionar**. 

10. En **Controles de acceso** selecciona **Sesión** y **0 controles seleccionados.**

11. Selecciona el cuadro **Usar Control de aplicaciones de acceso condicional**, deja el valor predeterminado **Solo supervisar** y selecciona **Seleccionar**.

12. En **Habilitar directiva**, selecciona **Activar** y después selecciona **Crear**.

#### Tarea 3: iniciar sesión en Forms y validar que el acceso condicional está supervisando

1. Inicia una nueva ventana de **exploración de InPrivate**.
2. Conéctese a [https://forms.microsoft.com](https://forms.microsoft.com).
3. Selecciona inicio de sesión en la esquina superior derecha de la página.
4. Inicia sesión como Pradeep Gupta.
   - Nombre de usuario: PradeepG@<<<your lab hoster provided domain>>>
   - Contraseña: la contraseña de la pestaña de recursos
5. Confirma que Pradeep tiene acceso y que tú recibes un nuevo mensaje:
   - Tu empresa supervisa el uso de esta aplicación.
6. Cierra la ventana de exploración de InPrivate.

### Ejercicio 2: configurar alertas en Microsoft Defender for Cloud Apps

#### Tarea 1: acceder a Microsoft Defender for Cloud Apps y crear un control de aplicaciones de acceso condicional

El registro de la aplicación establece una relación de confianza entre la aplicación y la plataforma de identidad de Microsoft. La confianza es unidireccional: la aplicación confía en la plataforma de identidad de Microsoft y no al revés.

1. Inicia sesión en [https://security.microsoft.com](https://security.microsoft.com) con una cuenta de administrador global.

1. En el menú de la izquierda, desplázate y selecciona **Directivas** en la sección **Aplicaciones en la nube** del menú de la izquierda.

1. En el menú **Directivas**, busca y selecciona **Administración de directivas**.

1. Seleccione **+ Crear directiva**. Selecciona **Directiva de acceso**.

1. Escribe un nombre para la directiva, como **Supervisar el acceso a Microsoft Forms.**

1. Deja la **Categoría** como **Control de acceso**.

1. En **Actividades que coinciden con todos los criterios siguientes**, selecciona en la lista desplegable **Compatible con Intune, Unido a Microsoft Entra Hybrid** y anula la selección **Unido a Microsoft Entra Hybrid**.

1. Selecciona la lista desplegable **Seleccionar aplicaciones**.  Selecciona **Microsoft Forms**.

1. Deja **Acciones** como **Prueba**.

1. En **Alertas**, deja **Crear una alerta...** activada y selecciona **Enviar alerta como correo electrónico**.

1. Escribe la dirección de correo electrónico del administrador del laboratorio y selecciona **Entrar** en el teclado.

1. Selecciona **Crear** para crear la directiva de acceso.

#### Tarea 2: iniciar sesión como Pradeep en Forms para desencadenar la actividad

1. Inicia una nueva ventana de **exploración de InPrivate**.
2. Conéctese a [https://forms.microsoft.com](https://forms.microsoft.com).
3. Selecciona inicio de sesión en la esquina superior derecha de la página.
4. Inicia sesión como Pradeep Gupta.
   - Nombre de usuario: PradeepG@<<<your lab hoster provided domain>>>
   - Contraseña: la contraseña de la pestaña de recursos
5. Confirma que Pradeep tiene acceso y que tú recibes un nuevo mensaje:
   - Tu empresa supervisa el uso de esta aplicación.
6. Cierra la ventana de exploración de InPrivate.

#### Tarea 3: revisar la actividad en Defender for Cloud Apps

1. Vuelve al explorador que ejecuta Defender for Cloud Apps.
2. Actualiza el explorador para asegurarte de que se descargan los datos más recientes.
3. En el menú **Investigar**, selecciona **Registro de actividad**.
4. Con la **aplicación: filtrar** elige **Microsoft Forms** de la lista.
5. Observa los registros de inicio de sesión de Pradeep.
