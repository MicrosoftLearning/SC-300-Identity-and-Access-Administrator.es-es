---
lab:
  title: '20: Implementación de la administración del acceso para aplicaciones'
  learning path: '03'
  module: Module 03 - Implement Access Management for Apps
---

# Laboratorio 20: Implementación de la administración del acceso para aplicaciones

### Tipo de inicio de sesión = Administración de Microsoft 365

## Escenario del laboratorio

Tu organización requiere que solo usuarios o grupos específicos tengan acceso a las aplicaciones empresariales. Tienes que asignar un usuario a una aplicación específica.

#### Tiempo estimado: 5 minutos

### Ejercicio 1: Configuración de una aplicación empresarial

#### Tarea 1: Adición de una aplicación al inquilino de Microsoft Entra

1. Inicia sesión en [https://entra.microsoft.com](https://entra.microsoft.com) con la cuenta de Administrador proporcionada.

2. Mira el menú en el lado izquierdo de la pantalla.

3. En el menú Identidad, en **Aplicaciones**, selecciona **Aplicaciones empresariales**.

4. En el panel Aplicaciones empresariales, selecciona **+ Nueva aplicación**.

    ![Imagen de pantalla que muestra la página Aplicaciones empresariales con la opción Nueva aplicación resaltada](./media/lp3-mod1-new-enterprise-application.png)

5. En la página Examinar la Galería de Microsoft Entra, en el cuadro **Buscar aplicación**, escribe **GitHub**.

    ![Imagen de pantalla que muestra la página Examinar la Galería de Microsoft Entra con el cuadro de búsqueda resaltado](./media/lp3-mod1-azure-ad-gallery-search.png)

6. En los resultados, selecciona **GitHub Enterprise Cloud - Enterprise Account**.

7. En la **GitHub Enterprise Cloud – Enterprise Account**, revisa la configuración y, después, selecciona **Crear**.

8. Una vez creada, se te redirigirá a la página de GitHub Enterprise Cloud - Enterprise Account.

#### Tarea 2: Asignación de usuarios a una aplicación

1. En la página GitHub Enterprise Cloud - Enterprise Account, en la página Información general, en **Introducción**, selecciona **1. Asignar usuarios y grupos**.

2. Como alternativa, en el panel de navegación izquierdo, en **Administrar**, puedes seleccionar **Usuarios y grupos**.

3. En la página Usuarios y grupos, en el menú, selecciona **+ Agregar usuario/grupo**.

4. En la página Agregar asignación, selecciona **Ninguno seleccionado** en la sección **Usuarios y grupos**.

5. En el panel Usuarios y grupos, selecciona Adele Vance y tu cuenta de administrador MOD.

6. Elige **Seleccionar**.

    ![Imagen de pantalla que muestra cómo agregar una asignación de cuenta de usuario a una aplicación con el botón Seleccionar resaltado ](./media/lp3-mod1-add-app-assignment.png)

7. Selecciona **Asignar**.

