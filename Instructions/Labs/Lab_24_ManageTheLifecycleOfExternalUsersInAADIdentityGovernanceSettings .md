---
lab:
  title: "24: administrar el ciclo de vida de los usuarios externos en la configuración de Azure\_AD Identity Governance"
  learning path: '04'
  module: Module 04 - Plan and Implement and Identity Governance Strategy
---

# Laboratorio 24: administración del ciclo de vida de los usuarios externos en la configuración de Azure AD Identity Governance  

## Escenario del laboratorio

Puede seleccionar lo que ocurre cuando un usuario externo, invitado a su directorio mediante una solicitud de paquete de acceso aprobada, ya no tiene ningún paquete de acceso asignado. Esto puede ocurrir si el usuario renuncia a todas sus asignaciones de paquetes de acceso o la asignación del último paquete de acceso expira. De forma predeterminada, cuando un usuario externo ya no tiene un paquete de acceso asignado, se le impide iniciar sesión en el directorio. Después de 30 días, la cuenta de usuario invitado se quitará del directorio.

#### Tiempo estimado: 5 minutos

### Ejercicio 1: configurar Azure AD Identity Governance

#### Tarea 1: administrar el ciclo de vida de los usuarios externos en la configuración de Azure AD Identity Governance

1. Inicia sesión en  [https://portal.azure.com](https://portal.azure.com) como Administrador global.

2. Se necesita una cuenta con administrador global o administrador de usuarios para completar estas tareas.

3. Abre Azure Active Directory y selecciona  **Identity Governance**.

4. En el menú de navegación izquierdo, en **Administración de derechos**, seleccione **Configuración**.

5. En el menú superior, seleccione **Editar**.

    ![Imagen de pantalla que muestra la página de configuración de Identity Governance con la opción Administración del ciclo de vida de los usuarios externos resaltada.](./media/lp4-mod1-manage-lifcycle-of-ext-users.png)

6. En la sección **Administración del ciclo de vida de los usuarios externos**, revise las diferentes opciones para los usuarios externos.

7. Cuando un usuario externo pierde la última asignación de un paquete de acceso, si desea bloquearlo para que no inicie sesión en este directorio, establezca **Impedir que los usuarios externos inicien sesión en este directorio** en **Sí**.

8. Si un usuario tiene bloqueada la capacidad de iniciar sesión en el directorio, no podrá volver a solicitar el paquete de acceso ni acceso adicional a este directorio. No configure el bloqueo de inicio de sesión si posteriormente necesitará solicitar acceso a otros paquetes de acceso.

9. Cuando un usuario externo pierde la última asignación de un paquete de acceso, si quiere quitar su cuenta de usuario invitado de este directorio, establezca **Quitar usuario externo** en **Sí**.

    **Nota**: la administración de derechos solo quita las cuentas a las que se ha invitado mediante la administración de derechos. Tenga en cuenta también que se impedirá que el usuario inicie sesión y se eliminará del directorio aunque dicho usuario se haya agregado a recursos del directorio que no eran asignaciones de paquetes de acceso. Si el invitado estaba presente en este directorio antes de recibir las asignaciones de paquetes de acceso, se mantendrá. Sin embargo, si se le invitó mediante una asignación de paquete de acceso y, después de invitarlo, se ha asignado a un sitio de OneDrive para la Empresa o SharePoint Online, se quitará.

10. Si quiere quitar la cuenta de usuario invitado de este directorio, puede establecer el número de días antes de que se quite. Si desea quitar la cuenta de usuario invitado en cuanto pierda la última asignación a cualquier paquete de acceso, establezca **Número de días antes de que se quite el usuario externo de este directorio** en **0**.

11. Si ha realizado algún cambio, seleccione **Guardar**.
