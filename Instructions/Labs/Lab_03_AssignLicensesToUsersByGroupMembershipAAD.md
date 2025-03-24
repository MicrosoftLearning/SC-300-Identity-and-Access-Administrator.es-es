---
lab:
  title: '03: Asignación de licencias con la pertenencia a grupos'
  learning path: '01'
  module: Module 01 - Implement an identity management solution
---

# Laboratorio 03: Asignación de licencias mediante la pertenencia a grupos

### Tipo de inicio de sesión = Inicio de sesión del inquilino de Microsoft 365 + E5

## Escenario del laboratorio

Tu organización ha decidido usar grupos de seguridad en Microsoft Entra ID para administrar licencias. Debes configurar un nuevo grupo de seguridad y asignar una licencia a ese grupo y comprobar que se han actualizado las licencias de miembro del grupo.

#### Tiempo estimado: 25 minutos

### Ejercicio 1: Creación de un grupo de seguridad y adición de un usuario

#### Tarea 1: Comprobación de si Delia Dennis tiene acceso a Office 365

1. Inicia una nueva ventana del explorador InPrivate.
2. Conéctate a [https://www.office.com](https://www.office.com).
3. Selecciona Iniciar sesión y conéctate como Delia Dennis.

   | **Configuración** | **Valor** |
   | :--- | :--- |
   | Nombre de usuario | DeliaD@`your domain name.com` |
   | Contraseña| Escribe la contraseña de usuario proporcionada para DeliaD. |

4. Debes conectarte al sitio web de Office.com, pero verás un mensaje que indica que no tienes una licencia.

   ![Imagen de pantalla del sitio web de Office.com que muestra que Delia Dennis inició sesión, pero no hay ninguna aplicación de Office disponible porque hay ninguna licencia asignada.](./media/delia-no-office-license.png)
    
5. Cierra la ventana del explorador.

#### Tarea 2: Creación de un grupo de seguridad en Microsoft Entra ID

1. Ve a [https://entra.microsoft.com](https://entra.microsoft.com).

2. En la navegación de la izquierda, en **Identidad**, selecciona **Grupos** y después **Todos los grupos**.
3. En la página Grupos, en el menú, selecciona **Nuevo grupo**.
4. Crea un grupo con esta información:

   | **Configuración**| **Valor**|
   | :--- | :--- |
   | Tipo de grupo| Seguridad|
   | Nombre del grupo| sg-SC300-O365|
   | Tipo de pertenencia| Asignada|
   | Propietarios| *Asigna tu propia cuenta de administrador como propietario del grupo*|

5. Selecciona el texto **Sin miembros seleccionados** en Miembros.
6. Selecciona **Delia Dennis** de la lista de usuarios.
7. Selecciona el botón **Seleccionar**.

   ![Imagen de pantalla que muestra la página de nuevo grupo con el tipo de grupo, el nombre del grupo, los propietarios y los miembros resaltados.](./media/lp1-mod2-create-group.png)

8. Selecciona el botón **Crear**.
9. Cuando termines, comprueba que el grupo denominado **sg-SC300-O365** aparece en la lista **Todos los grupos**.

#### Tarea 3: Adición de una licencia de Office a sg-SC300-O365

**Sugerencia de laboratorio**: tienes que agregar y quitar licencias a través del Centro de administración de Microsoft 365. Este es un cambio relativamente nuevo.

1. Abre una nueva pestaña en tu explorador.

2. Conéctate al Centro de administración de Microsoft 365 en http://admin.microsoft.com.

3. Inicia sesión con tu cuenta de administrador si se te solicita.

4. En el menú de la izquierda, selecciona **Facturación** y después selecciona **Licencias**.

5. Selecciona la licencia **Office 365 E3** de la lista.

6. Selecciona la pestaña **Grupos** en la pantalla de licencias.

7. Elige el elemento **+ Asignar licencias**.

8. Busca el grupo **sg-SC300-O365** y selecciónalo de la lista.

8. Una vez que hayas agregado el grupo, selecciona **Asignar**.
 
9. Cierra el mensaje de confirmación.

10. Vuelve a la pestaña del explorador con el **Centro de administración Microsoft Entra** abierto.

11. Vuelve a **Todos los grupos** en el menú de la izquierda, en **Identidad**, selecciona **Grupos**

12. En la página Grupos, selecciona **sg-SC300-O365**.

13. En el panel de navegación izquierdo, selecciona **Licencias.**

14. Observa que se ha asignado la licencia Office 365 E3.

15. Puedes salir de la pantalla de licencia.

#### Tarea 4: Confirmación de la licencia de Office 365

1. Inicia una nueva ventana del explorador InPrivate.
2. Conéctate a [https://www.office.com](https://www.office.com).
3. Selecciona Iniciar sesión y conéctate como Delia Dennis.

   | **Configuración**| **Valor**|
   | :--- | :--- |
   | Nombre de usuario | DeliaD@`your domain name.com`|
   | Contraseña| En Contraseña, escribe la contraseña proporcionada.  |

4. Debes conectarte al sitio web de Office.com y no ver ningún mensaje con respecto a la licencia. Todas las aplicación de Office están disponibles a la izquierda.

   ![Imagen de pantalla del sitio web de Office.com con Delia Dennis que inició sesión con las aplicaciones de office disponibles, ya que hay asignada una licencia.](./media/delia-office-license.png)
    
5. Cierra la ventana del explorador.

### Ejercicio 2: Creación de un grupo de Microsoft 365 en Microsoft Entra ID

#### Tarea 1: Creación del grupo

Parte de tus tareas como administrador de Microsoft Entra es crear diferentes tipos de grupos. Debes crear un nuevo grupo de Microsoft 365 para el departamento de ventas de tu organización.

1. Ve a [https://entra.microsoft.com]( https://entra.microsoft.com).

2. En la navegación de la izquierda, en **Identidad**, selecciona **Grupos** y después selecciona **Todos los grupos**

3. En la página Grupos, en el menú, selecciona **Nuevo grupo**.

4. Crea un grupo con esta información:

   | **Configuración**| **Valor**|
   | :--- | :--- |
   | Tipo de grupo| Microsoft 365|
   | Nombre del grupo| Northwest Sales|
   | Tipo de pertenencia| Asignada|
   | Propietarios| *Asigna tu propia cuenta de administrador como propietario del grupo*|
   | Miembros| **Alex Wilber** y **Bianca Pisani**|

   ![Imagen de pantalla que muestra la página de nuevo grupo con el tipo de grupo, el nombre del grupo, los propietarios y los miembros resaltados.](./media/lp1-mod2-create-o365-group.png)

5. Cuando termines, comprueba que el grupo denominado **Northwest Sales** aparece en la lista **Todos los grupos**.

### Ejercicio 3: Creación de un grupo dinámico con todos los usuarios como miembros

#### Tarea 1: Creación del grupo dinámico

A medida que crece tu empresa, la administración manual de grupos se hace demasiado lenta. Desde la estandarización del directorio, puedes sacar partido a los grupos dinámicos. Debes crear un nuevo grupo dinámico para asegurarte de que esté todo listo para la creación de grupos dinámicos en producción.

1. Inicie sesión en [https://entra.microsoft.com](https://entra.microsoft.com) con la cuenta de administrador proporcionada. Necesitas al menos el rol de administrador de usuarios en el inquilino.

2. Selecciona **Identidad**.

3. En **Grupos**, selecciona **Todos los grupos** y después selecciona **Nuevo grupo**.

4. En la página Nuevo grupo, en **Tipo de grupo**, selecciona **Seguridad**.

5. En el cuadro del **Nombre del grupo**, escribe **SC300-myDynamicGroup**.

6. Selecciona el menú **Tipo de pertenencia** y, luego, selecciona **Usuario dinámico**.

7. Selecciona un **Propietario** para el grupo.

7. En **Miembros usuarios dinámicos**, seleccione **Agregar consulta dinámica**.

8. A la derecha, sobre el cuadro **Sintaxis de regla**, selecciona **Editar**.

9. En el panel de edición de sintaxis de regla, escribe esta expresión en el cuadro **Sintaxis de regla**:

   ```powershell
   user.objectId -ne null
   ```

   **Advertencia:** el `user.objectId` distingue mayúsculas de minúsculas.

10. Selecciona **Aceptar**. La regla aparece en el cuadro Sintaxis de regla.

   ![Imagen de pantalla que muestra la página de reglas de pertenencia dinámica a grupos con sintaxis de regla resaltada.](./media/lp1-mod3-dynamic-group-membership-rule.png)

11. Selecciona **Guardar**. En el nuevo grupo dinámico se incluirán ahora los usuarios invitados de B2B y los usuarios miembros.

12. En la página Nuevo grupo, selecciona **Crear** para crear el grupo.

#### Tarea 2: Comprobación de que se han agregado los miembros

**Nota:** el rellenado de pertenencia dinámica a grupos puede tardar hasta 15 minutos.

1. Selecciona **Inicio**`Microsoft Entra admin center`.
2. Inicia **Identidad**.
3. En el menú **Grupos**, selecciona **Todos los grupos**.
4. En el cuadro de filtro, escribe **SC300** y se mostrará el grupo recién creado.
5. Selecciona **SC300-myDynamicGroup** para abrir el grupo.
6. Observa que muestra que contiene más de 30 **miembros directos*.
7. Selecciona **Miembros** en el menú **Administrar**.
8. Revisa los miembros.

#### Tarea 3: Experimentación con reglas alternativas

1. Prueba a crear un grupo solo con usuarios **Invitados**:

   - (user.objectId -ne null) y (user.userType -eq "Guest")

2. Prueba a crear un grupo solo con **miembros** de los usuarios de Microsoft Entra.

   - (user.objectId -ne null) y (user.userType -eq "Member")

**Sugerencia de laboratorio**: si recibes un mensaje de error al crear un grupo que menciona un operador no válido, comprueba la ortografía del operador.  Nota I en objectId y T en userType son letras mayúsculas.
