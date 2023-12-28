---
lab:
  title: '21: consentimiento administrativo para todos los inquilinos a una aplicación'
  learning path: '03'
  module: Module 03 - Implement Access Management for Apps
---

# Laboratorio 21: consentimiento administrativo para todos los inquilinos a una aplicación

## Escenario del laboratorio

Para las aplicaciones que haya desarrollado la organización, o bien las que se hayan registrado directamente en el inquilino de Azure AD, puede conceder el consentimiento del administrador para todo el inquilino desde los Registros de aplicaciones en Azure Portal.

#### Tiempo estimado: 15 minutos

### Ejercicio 1: consentimiento de administrador

#### Tarea 1: conceder el consentimiento de administrador en Registros de aplicaciones

   **Advertencia**: la concesión del consentimiento del administrador para todos los inquilinos a una aplicación permitirá que la aplicación y el editor accedan a los datos de la organización. Antes de conceder el consentimiento, revise con atención los permisos que solicita la aplicación.

El rol de administrador global es necesario para proporcionar el consentimiento del administrador para los permisos de aplicación en Microsoft Graph API.

1. En un ejercicio anterior creó una aplicación denominada Aplicación de demostración. Si es necesario, en Microsoft Azure, ve a **Azure Active Directory**, selecciona **Registros de aplicaciones** y después, **Aplicación de demostración**.


2. En la página de la **Aplicación de demostración**, busca, copia y guarda los valores de **Id. de aplicación (cliente)** y de **Id. de directorio (inquilino)** para poder usarlos más adelante.

    >**Nota**: **la aplicación de demostración** se ha creado en los laboratorios anteriores. Estos laboratorios son un requisito previo.

    ![Captura de pantalla que muestra la página Aplicación de demostración con el Id. de directorio resaltado](./media/lp3-mod3-demo-app-directory-id.png)

3. En el panel de navegación izquierdo, en **Administrar**, seleccione **Permisos de API**.

4. En **Permisos configurados**, seleccione **Conceder consentimiento del administrador**.

    ![Captura de pantalla que muestra la página de permisos de API con la opción Conceder consentimiento del administrador para Contoso resaltada](./media/lp3-mod3-api-permissions-admin-consent.png)

5. Revise el cuadro de diálogo y, a continuación, seleccione **Sí**.

   **Advertencia**: la concesión del consentimiento del administrador para todos los inquilinos a través de Registros de aplicaciones revocará todos los permisos concedidos previamente a todos los inquilinos. Los permisos que los usuarios hayan concedido previamente en su propio nombre no se verán afectados.

#### Tarea 2: conceder el consentimiento de administrador en las aplicaciones empresariales

Puede conceder el consentimiento del administrador para todo el inquilino a través de Aplicaciones empresariales si la aplicación ya se ha aprovisionado en el inquilino.

1. En Microsoft Azure, vaya a **Azure Active Directory > Aplicaciones de empresa > Aplicación de demostración.**

2. En la página **Aplicación de demostración**, en el panel de navegación izquierdo, selecciona **Permisos** en la sección **Seguridad**.

3. En **Permisos**, seleccione **Conceder consentimiento del administrador**.

    ![Captura de pantalla que muestra la página de permisos de la aplicación de demostración con la opción Conceder consentimiento del administrador para Contoso resaltada.](./media/lp3-mod3-grant-admin-consent-in-enterprise-app.png)

   **Advertencia**: la concesión del consentimiento del administrador para todos los inquilinos a través de Registros de aplicaciones revocará todos los permisos concedidos previamente a todos los inquilinos. Los permisos que los usuarios hayan concedido previamente en su propio nombre no se verán afectados.

4. Cuando se le solicite, inicie sesión con la cuenta de administrador global.

5. En el cuadro de diálogo **Permisos solicitados**, revise la información y seleccione **Aceptar**.
