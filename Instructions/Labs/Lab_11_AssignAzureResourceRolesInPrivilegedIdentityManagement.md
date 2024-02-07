---
lab:
  title: '11: asignar roles de recursos de Azure en Privileged Identity Management'
  learning path: '02'
  module: Module 02 - Implement an authentication and access management solution
---

# Laboratorio 11: asignar roles de recursos de Azure en Privileged Identity Management

**Nota:** este laboratorio requiere un pase para Azure. Consulta el laboratorio 00 para obtener instrucciones.

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

#### Tarea 1: asignar roles de recursos de Azure

1. Inicia sesión en [https://entra.microsoft.com](https://entra.microsoft.com) con una cuenta de administrador global.

2. Busca y luego selecciona **Privileged Identity Management.**

3. En la página Privileged Identity Management, en la navegación izquierda, selecciona **Recursos de Azure.**

4. En el menú superior, seleccione **Detectar recursos**.

5. En la página Detección de recursos de Azure, selecciona tu suscripción.

   ![Imagen de pantalla que muestra la página de detección de recursos de Azure con las opciones Suscripción y Administrar recursos resaltadas](./media/lp4-mod3-pim-azure-resource-management.png)

6. En la página **Información general**, revisa la información.

   ![Imagen de pantalla que muestra el recurso de Azure agregado recientemente](./media/lp4-mod3-pim-az-resource-overview.png)

7. En el menú de navegación izquierdo, en **Administrar**, seleccione **Roles** para ver la lista de roles de los recursos de Azure.

8. En el menú superior, seleccione **+ Agregar asignaciones**.

9. En la página Agregar asignaciones, selecciona el menú **Seleccionar rol** y después **Colaborador de servicios de API Management.**

10. En **Seleccionar miembros**, seleccione **No hay miembros seleccionados**.

11. En la sección Seleccionar **Miriam Graham** de tu organización a quien se le asignará el rol.  Después elige **Seleccionar**.

12. Seleccione **Siguiente**.

13. En la pestaña **Configuración**, en la lista **Tipo de asignación**, seleccione **Elegible**.

   - Las asignaciones tipo **Apto** requieren que el miembro del rol realice una acción para usar el rol. Entre las acciones se puede incluir realizar una comprobación de autenticación multifactor (MFA), proporcionar una justificación de negocios o solicitar la aprobación de los aprobadores designados.

   - Las asignaciones **activas** no requieren que el miembro realice ninguna acción para usar el rol. Los miembros asignados como activos siempre tienen privilegios asignados al rol.

14. Especifique una duración de asignación cambiando las fechas y horas de inicio y finalización.

15. Cuando termine, seleccione **Asignar**.

16. Una vez creada la nueva asignación de roles, se muestra una notificación de estado.

#### Tarea 2: actualizar o eliminar una asignación de roles existente

Siga estos pasos para actualizar o quiotar una asignación de roles existente.

1. Abra **Microsoft Entra Privileged Identity Management**.

2. Seleccione **Azure resources** (Recursos de Azure).

3. Selecciona la suscripción que quieres administrar para abrir su página de información general.

4. En **Administrar**, seleccione **Asignaciones**.

5. En la pestaña **Asignaciones aptas**, en la columna Acción, revisa las opciones disponibles.

6. Seleccione **Quitar**.

7. En el cuadro de diálogo **Quitar**, revise la información y, a continuación, seleccione **Sí**.
