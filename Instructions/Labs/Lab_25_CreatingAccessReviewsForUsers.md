---
lab:
  title: '25: crear revisiones de acceso para usuarios internos y externos'
  learning path: '04'
  module: Module 04 - Plan and Implement and Identity Governance Strategy
---

# Laboratorio 25: crear revisiones de acceso para usuarios internos y externos  

## Escenario del laboratorio

El acceso de usuario con privilegios debe revisarse periódicamente de forma similar.  Dado que se trata de asignaciones de acceso elevadas, la revisión de estas debe realizarse de forma coherente, tal como se identifique en la empresa.Deben eliminarse las asignaciones con privilegios sin usar y superfluas.  La eliminación automatizada también debe configurarse para los usuarios que ya no estén en la empresa o que hayan cambiado de departamento dentro de la empresa.

#### Tiempo estimado: 5 minutos

### Ejercicio 1: crear una revisión de acceso interna

#### Tarea: crear una revisión de acceso nueva

1. Inicia sesión en  [https://entra.microsoft.com](https://entra.microsoft.com) como Administrador global.

2. Las revisiones de acceso pueden administrar el ciclo de vida de acceso.En **Microsoft Entra ID**, busca **Gobernanza de identidades** y después selecciona **Revisiones de acceso**.

3. Seleccione **Nueva revisión de acceso**.

4. En el cuadro **Selecciona qué revisar** selecciona **Equipos + grupos** en el menú desplegable.

5. Selecciona **Seleccionar Teams + grupos** y elige el grupo  **Ventas y marketing** de la lista y presiona **Seleccionar**.

6. Establece el **Ámbito** en **Todos los usuarios**.

7. Selecciona **Siguiente: Revisiones** para avanzar en el asistente.

8. El siguiente paso es determinar los revisores.  Estos revisores pueden ser los propios miembros y revisarse a sí mismos, pero también pueden ser los supervisores si se tiene que revisar el acceso de todo un departamento. También puedes establecer la acción cuando un revisor no responde para quitar automáticamente ese acceso con privilegios del miembro.

9. Elige un revisor y revisa la opción de periodicidad.  Luego, seleccione **Configuración**.

10. La configuración avanzada te permite incluir un mensaje como parte de la revisión.

11. Cambia a la pestaña **Revisar y crear** para finalizar la revisión de acceso.

12. Pon a la revisión de acceso el nombre **SC300 prueba de revisión de acceso**.

13. En la parte inferior de la página, seleccione **Crear**.

    **Nota:** Cuando se cree la revisión de acceso, la lista de revisión de acceso se rellenará con los roles y propietarios de las revisiones.

14. Los miembros que se están revisando recibirán un correo electrónico cuando se inicie la revisión.

15. Al seleccionar una revisión de acceso de uno de los roles, se proporcionará el estado de estas revisiones de acceso.
