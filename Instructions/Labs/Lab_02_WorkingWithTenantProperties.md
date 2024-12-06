---
lab:
  title: '02: trabajar con propiedades de inquilino'
  learning path: '01'
  module: Module 01 - Implement an Identity Management Solution
---

# Laboratorio 02: trabajar con propiedades de inquilino

### Tipo de inicio de sesión = Administración de Microsoft 365

## Escenario del laboratorio

Debes identificar y actualizar las distintas propiedades asociadas a tu inquilino.

#### Tiempo estimado: 15 minutos

### Ejercicio 1: Crear subdominios personalizados 

#### Tarea 1: crear nombre de subdominio personalizado

Usarías Microsoft Entra ID para crear un dominio que hayas adquirido.  Si deseas crear un subdominio para dividir el dominio .onmicrosoft.com existente, debes usar el Centro de administración de Microsoft 365.

1. Ve a [https://entra.microsoft.com](https://entra.microsoft.com) e inicia sesión con una cuenta de administrador global para el directorio.

1. En el menú **Identidad**, usa la opción **Mostrar más** de la parte inferior.

1.  Abra el menú **Configuración**, seleccione **Nombres de dominio**.

1. Seleccione **+ Agregar dominio personalizado**.

1. En el campo **Nombre de dominio personalizado**, crea un subdominio personalizado para el inquilino de laboratorio colocando **ventas** delante del nombre de dominio**onmicrosoft.com**.  El formato será similar a lo siguiente:

    ```
    mydomain.com
    ```

1. **Nota**: se le pedirá que abra el Centro de administración de Microsoft 365 para completar esta acción.

1. Selecciona **Agregar dominio** para agregar el subdominio.

1. Escribe el nombre del subdominio `sales.tenantname.onmicrosoft.com` en el cuadro de diálogo.

1. Selecciona el botón **Usar este dominio** en la parte inferior de la pantalla.

1. Selecciona el botón **Cerrar** cuando se abra la siguiente pantalla.  Para este laboratorio no configuraremos el DNS.

### Ejercicio 2: cambiar el nombre de visualización del inquilino

#### Tarea 1: establecer el nombre del inquilino y el contacto técnico

1. En el Centro de administración de Microsoft Entra, abre el menú **Identidad**.

1. En el panel de navegación izquierdo, selecciona **Información general** y luego selecciona **Propiedades**.

1. Cambia las propiedades del inquilino de **Nombre** y **Contacto técnico** en el cuadro de diálogo.

    | **Configuración** | **Valor** |
    | :--- | :--- |
    | Nombre | Contoso Marketing |
    | Contacto técnico | `your Global admin account` |

1. Seleccione **Guardar** para actualizar las propiedades del inquilino.

   **Observarás que el nombre cambia inmediatamente después de completar la operación de guardar.**

#### Tarea 2: revisar el país o la región y otros valores asociados a tu inquilino

1. En el menú **Identidad**, selecciona **Información general** y luego selecciona **Propiedades**.

2. En **Propiedades de inquilino**, busque **País o región** y revise la información.

    **IMPORTANTE**: el país o la región se especifican cuando se crea el inquilino. Esta configuración no puede modificarse más tarde.

3. En la pantalla **Propiedades**, en **Propiedades de inquilino**, busca **Ubicación** y revisa la información.

    ![Imagen de pantalla que muestra la página Propiedades de Azure Active Directory con la configuración de País o región y Ubicación resaltada.](./media/azure-active-directory-properties-country-location.png)

#### Tarea 3: buscar el Id. de inquilino

Las suscripciones de Azure tienen una relación de confianza con Microsoft Entra ID. Se confía en Microsoft Entra ID para autenticar los usuarios, servicios y dispositivos de la suscripción. Cada suscripción tiene un identificador de inquilino asociado y hay varias maneras de encontrarlo.

1. Abre el Centro de administración de Microsoft Entra[https://entra.microsoft.com](https://entra.microsoft.com)

1. En el menú **Identidad**, selecciona **Información general** y luego selecciona **Propiedades**.

1. En **Propiedades de inquilino**, busque **Identificador de inquilino**. Este es el identificador único de inquilino.

    ![Imagen de pantalla que muestra la página Propiedades de inquilino con el cuadro Identificador de inquilino resaltado](./media/portal-tenant-id.png)

### Ejercicio 3: Configuración de tu información de privacidad

#### Tarea 1: Adición de la información de privacidad en Microsoft Entra ID, incluidos el contacto de privacidad global y la dirección URL de declaración de privacidad

Microsoft recomienda agregar su contacto de privacidad global y la declaración de privacidad de su organización, de modo que los empleados internos e invitados externos puedan revisar las directivas. Dado que las declaraciones de privacidad se crean de forma única y específica para cada negocio, es recomendable ponerse en contacto con un abogado para obtener ayuda.

   **NOTA**: Para obtener información sobre cómo ver o eliminar datos personales, consulta https://docs.microsoft.com/microsoft-365/compliance/gdpr-dsr-azure[](https://docs.microsoft.com/microsoft-365/compliance/gdpr-dsr-azure). Para obtener más información acerca de RGPD, consulta la [https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

Puede agregar la información de privacidad de su organización en el área de  **Propiedades**  de Microsoft Entra ID. Para acceder al área de propiedades y agregar la información de privacidad:

1. En el menú **Identidad**, selecciona **Información general** y luego selecciona **Propiedades**.

    ![Imagen de pantalla que muestra las propiedades de inquilino con los cuadros Contacto técnico, Contacto global y Declaración de privacidad resaltados](./media/properties-area.png)

2. Agregar la información de privacidad de sus empleados:

- **Contacto de privacidad global** - `AllanD@`**tu dominio del laboratorio de Azure**
     - Allan Deyoung es un usuario integrado en el inquilino del laboratorio de Azure que trabaja como un administrador de TI, lo usaremos como contacto de privacidad.
     - Esta persona también es quien se pone en contacto con Microsoft si se produce una vulneración de datos. Si no aparece ninguna persona aquí, Microsoft se pone en contacto con los administradores globales.

- **URL de la declaración de privacidad** -  <https://github.com/MicrosoftLearning/SC-300-Identity-and-Access-Administrator/blob/master/Allfiles/Labs/Lab2/SC-300-Lab_ContosoPrivacySample.pdf>

     - Se proporciona el PDF de privacidad de ejemplo en el directorio de laboratorios.
     - Escribe el vínculo al documento de tu organización que describe cómo administra tu organización, la privacidad de los datos de los huéspedes internos y externos.

    **IMPORTANTE** Si no incluyes tu propia declaración de privacidad o tu contacto de privacidad, los invitados externos verán el texto en el cuadro de diálogo Permisos de revisión que dice:  **<tu nombre de organización\>** no ha facilitado vínculos de tus términos que puedas revisar. Por ejemplo, un usuario invitado verá este mensaje cuando reciba una invitación para acceder a una organización a través de la colaboración B2B.

    ![Cuadro de diálogo Permisos de revisión de colaboración B2B con el mensaje](./media/active-directory-no-privacy-statement-or-contact.png)

3. Seleccione **Guardar**.

#### Tarea 2: comprobar tu declaración de privacidad

1. Vuelve a la pantalla principal de Azure: panel.
2. En la esquina superior derecha de la interfaz de usuario, selecciona tu nombre de usuario.
3. Selecciona **Ver cuenta** en el menú desplegable.

     **Se abrirá automáticamente una nueva pestaña del explorador.**

4. Selecciona **Configuración y privacidad** en el menú de la izquierda.
5. Seleccione **Privacidad**.
6. En **Aviso de la organización**, selecciona el elemento **Ver** junto a la declaración de privacidad de la organización de Contoso Marketing.

     **Se abrirá una nueva pestaña del navegador en la que se mostrará el archivo PDF de privacidad que has vinculado.**

7. Revisa la declaración de privacidad de muestra.
8. Cierra la pestaña del explorador con el PDF.
9. Cierra la pestaña del explorador en la que se muestran los elementos **Mi cuenta**.
