---
lab:
  title: 25 - Creación de revisiones de acceso para usuarios internos y externos
  learning path: '04'
  module: Module 04 - Plan and Implement and Identity Governance Strategy
---

# Laboratorio 25: creación de revisiones de acceso para usuarios internos y externos  

## Escenario del laboratorio

El acceso de usuario con privilegios debe revisarse con regularidad de forma similar.Dado que se trata de asignaciones de acceso elevado, la revisión de estas debe realizarse de forma coherente, tal como identifica en la empresa.Deben eliminarse las asignaciones privilegiadas que no se usen o sean innecesarias.La eliminación automatizada también debe configurarse para los usuarios que ya no estén en la empresa o que hayan cambiado de departamento dentro de la empresa.

#### Tiempo estimado: 5 minutos

### Ejercicio 1: creación de una revisión de acceso interna

#### Tarea: crear una revisión de acceso nueva

1. Inicia sesión en  [https://portal.azure.com](https://portal.azure.com) como Administrador global.

2. Las revisiones de acceso pueden administrar el ciclo de vida de acceso.En **Azure Active Directory**, busca **Identity Governance** en el menú **Administrar**.  En **Identity Goverance**, selecciona **Revisiones de acceso**.

3. Seleccione **Nueva revisión de acceso**.

4. En el cuadro **Selecciona qué revisar** elige **Equipos + grupos** del menú desplegable.

5. Selecciona **Seleccionar equipos + grupos** y elige el grupo **Ventas y Marketing** de la lista, y pulsa **Seleccionar**.

6. Establece el **Ámbito** en **Todos los usuarios**.

7. Selecciona la opción **Revisiones** para avanzar en el asistente.

8. El siguiente paso es determinar los revisores.Estos revisores pueden ser miembros para realizar una revisión automática o pueden ser los supervisores para revisar el acceso de un departamento. También puedes establecer la acción cuando un revisor no responde para quitar ese acceso con privilegios automáticamente.

9. Elige un revisor y revisa la opción de periodicidad.  Luego, seleccione **Configuración**.

10. La configuración avanzada te permite insertar un mensaje como parte de la revisión.

11. Selecciona **Revisar y crear** para finalizar la revisión de acceso.

12. Nombra la revisión de acceso **SC300 Access Review Test**.

13. Seleccione **Crear**. Cuando se cree la revisión de acceso, la lista de revisión de acceso se rellenará con los roles y propietarios de las revisiones.

14. Los miembros que se están revisando recibirán un correo electrónico cuando se inicie la revisión.

15. Al seleccionar una revisión de acceso de uno de los roles, se te proporcionará el estado de estas revisiones de acceso.
