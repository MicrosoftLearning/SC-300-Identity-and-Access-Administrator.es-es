---
lab:
  title: '11: Asignación de roles de recursos de Azure en Privileged Identity Management'
  learning path: '02'
  module: Module 02 - Implement an authentication and access management solution
---

# Laboratorio 11: Asignación de roles de recursos de Azure en Privileged Identity Management

### Tipo de inicio de sesión = Inicio de sesión de recurso de Azure

## Escenario del laboratorio

Microsoft Entra Privileged Identity Management (PIM) puede administrar los roles de recurso integrados de Azure, así como los roles personalizados, entre ellos los siguientes:

- Propietario
- Administrador de acceso de usuario
- Colaborador
- Administrador de seguridad
- Administrador de seguridad

Necesitas hacer que un usuario sea elegible para un rol de recurso de Azure.

#### Tiempo estimado: 10 minutos

### Ejercicio 1: PIM con recursos de Azure

#### Tarea 1: Asignación de roles de recursos de Azure

1. Inicia sesión en [https://entra.microsoft.com](https://entra.microsoft.com) con la cuenta de Administrador.

2. Busque y, a continuación, seleccione **Microsoft Entra Privileged Identity Management.**

3. En la página Privileged Identity Management, en la navegación izquierda, selecciona **Recursos de Azure.**

**Sugerencia de laboratorio**: los pasos siguientes se escribieron para la experiencia de recursos heredados de Azure.  Puedes cambiar a la experiencia antigua en la parte superior de la pantalla. O bien, puedes completar el ejercicio en la nueva experiencia sin el paso a paso.

4. En la lista desplegable Suscripciones, elige el elemento Suscripción de MOC#####. Después, en la parte inferior de la pantalla, selecciona **Administrar recursos**.

5. En la página Detección de recursos de Azure, selecciona tu suscripción.

6. En la página **Información general**, revisa la información.

   ![Imagen de pantalla que muestra el recurso de Azure agregado recientemente](./media/lp4-mod3-pim-az-resource-overview.png)

   **Sugerencia de laboratorio**: debido a la naturaleza del entorno de laboratorio, no verás ningún recurso. Consulta la imagen para obtener un ejemplo.

7. En el menú de navegación izquierdo, en **Administrar**, selecciona **Roles** para ver la lista de roles de los recursos de Azure.

8. En el menú superior, selecciona **+ Agregar asignaciones**.

9. En la página Agregar asignaciones, selecciona el menú **Seleccionar rol** y después **Colaborador de servicios de API Management.**

10. En **Seleccionar miembros**, selecciona **No hay miembros seleccionados**.

11. En Seleccionar un miembro o grupo, busca los roles de administrador **Usuario1-######@LODSPRODMCA.onmicrosoft.com** de tu organización a los que se asignará el rol.  Después elige **Seleccionar**.

12. Selecciona **Siguiente**.

13. En la pestaña **Configuración**, en la lista **Tipo de asignación**, selecciona **Elegible**.

   - Las asignaciones tipo **Apto** requieren que el miembro del rol realice una acción para usar el rol. Entre las acciones se puede incluir realizar una comprobación de autenticación multifactor (MFA), proporcionar una justificación de negocios o solicitar la aprobación de los aprobadores designados.

   - Las asignaciones **activas** no requieren que el miembro realice ninguna acción para usar el rol. Los miembros asignados como activos siempre tienen privilegios asignados al rol.

14. Especifica una duración de asignación cambiando las fechas y horas de inicio y finalización.

15. Cuando termine, selecciona **Asignar**.

16. Una vez creada la nueva asignación de roles, se muestra una notificación de estado.

#### Tarea 2: actualización o eliminación de una asignación de roles existente

**Nota:** debido a la seguridad aplicada en este entorno de laboratorio, no puedes completar estos pasos.  Revisa los pasos descritos en la interfaz de usuario, pero no podrás aplicar los cambios.  Estamos trabajando activamente para encontrar una solución alternativa a esto.

Sigue estos pasos para actualizar o quitar una asignación de roles existente.

1. Abre **Microsoft Entra Privileged Identity Management**.

2. Seleccione **Recursos de Azure**.

3. Selecciona la suscripción que quieres administrar para abrir su página de información general.

4. En **Administrar**, seleccione **Asignaciones**.

5. En la pestaña **Asignaciones aptas**, en la columna Acción, revisa las opciones disponibles.

6. Seleccione **Quitar**.

7. En el cuadro de diálogo **Quitar**, revise la información y, a continuación, seleccione **Sí**.
