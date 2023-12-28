---
lab:
  title: '11: asignar roles de recursos de Azure en Privileged Identity Management'
  learning path: '02'
  module: Module 02 - Implement an authentication and access management solution
---

# Laboratorio 11: asignar roles de recursos de Azure en Privileged Identity Management

**Nota:** este laboratorio requiere un pase para Azure. Consulta el laboratorio 00 para obtener indicaciones.

## Escenario del laboratorio

Azure Active Directory (Azure AD) Privileged Identity Management (PIM) puede administrar los roles de recurso integrados de Azure, así como los roles personalizados, entre ellos los siguientes:

- Propietario
- Administrador de acceso de usuario
- Colaborador
- Administrador de seguridad
- Administrador de seguridad

Necesitas hacer que un usuario sea elegible para un rol de recurso de Azure.


#### Tiempo estimado: 10 minutos

### Ejercicio 1: PIM con recursos de Azure

#### Tarea 1: asignar roles de recursos de Azure

1. Inicia sesión en [https://portal.azure.com](https://portal.azure.com) con una cuenta de administrador global.

2. Busque y, a continuación, seleccione **Azure AD Privileged Identity Management.**

3. En la página Privileged Identity Management, en la navegación izquierda, selecciona **Recursos de Azure.**

4. En el menú superior, seleccione **Detectar recursos**.

5. En la página Recursos de Azure: detección, selecciona tu suscripción y después, en el menú superior, selecciona **Administrar recurso**.

   ![Imagen de pantalla que muestra la página de detección de recursos de Azure con las opciones Suscripción y Administrar recursos resaltadas](./media/lp4-mod3-pim-azure-resource-management.png)

6. En el cuadro de diálogo **Incorporación de recursos seleccionados para administración**, revise la información y seleccione **Aceptar**.

7. Cuando se complete la incorporación, cierra la página Recursos de Azure: detección.

8. En la página de recursos de Azure, selecciona la suscripción.

   ![Imagen de pantalla que muestra el recurso de Azure agregado recientemente](./media/lp4-mod3-pim-az-resource-overview.png)

9. En el menú de navegación izquierdo, en **Administrar**, seleccione **Roles** para ver la lista de roles de los recursos de Azure.

10. En el menú superior, seleccione **+ Agregar asignaciones**.

11. En la página Agregar asignaciones, selecciona el menú **Seleccionar rol** y después **Colaborador de servicios de API Management.**

12. En **Seleccionar miembros**, seleccione **No hay miembros seleccionados**.

13. Selecciona **Miriam Graham** de tu organización, a quien se le asignará el rol.  Después elige **Seleccionar**.

14. Seleccione **Siguiente**.

15. En la pestaña **Configuración**, en la lista **Tipo de asignación**, seleccione **Elegible**.

   - Las asignaciones tipo **Apto** requieren que el miembro del rol realice una acción para usar el rol. Entre las acciones se puede incluir realizar una comprobación de autenticación multifactor (MFA), proporcionar una justificación de negocios o solicitar la aprobación de los aprobadores designados.

   - Las asignaciones **activas** no requieren que el miembro realice ninguna acción para usar el rol. Los miembros asignados como activos siempre tienen privilegios asignados al rol.

16. Especifique una duración de asignación cambiando las fechas y horas de inicio y finalización.

17. Cuando termine, seleccione **Asignar**.

18. Una vez creada la nueva asignación de roles, se muestra una notificación de estado.

#### Tarea 2: elegir o eliminar una asignación de roles existente

Siga estos pasos para actualizar o quiotar una asignación de roles existente.

1. Abra **Azure AD Privileged Identity Management**.

2. Seleccione **Azure resources** (Recursos de Azure).

3. Selecciona el recurso que deseas administrar para abrir su página información general.

4. En **Administrar**, seleccione **Asignaciones**.

5. En la pestaña **Roles elegibles**, en la columna Acción, revise las opciones disponibles.

6. Seleccione **Quitar**.

7. En el cuadro de diálogo **Quitar**, revise la información y, a continuación, seleccione **Sí**.
