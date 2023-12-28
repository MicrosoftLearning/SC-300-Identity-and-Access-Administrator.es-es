---
lab:
  title: '17: detectar aplicaciones de Defender for Cloud Apps y aplicar restricciones'
  learning path: '03'
  module: Module 03 - Implement Access Management for Apps
---

# Laboratorio 17: detectar aplicaciones de Defender for Cloud Apps y aplicar de restricciones

## Escenario del laboratorio

Microsoft Defender for Cloud Apps utiliza registros del tráfico de red para identificar las aplicaciones a las que acceden los usuarios.Los registros de tráfico de firewalls locales proporcionarán un informe de instantáneas sobre las aplicaciones más comunes y los usuarios que acceden a estas aplicaciones.El tráfico desde dispositivos administrados se introducirá en el panel de información general de detección de Microsoft Defender for Cloud Apps.

#### Tiempo estimado: 10 minutos

### Ejercicio 1: detectar Defender for Cloud Apps

#### Tarea 1: detectar aplicaciones en Defender for Cloud Apps

1. Inicia sesión en [https://security.microsoft.com](https://security.microsoft.com)  con una cuenta de administrador global.

1. En el menú izquierdo, desplázate hasta el encabezado denominado **Cloud Apps** y haz clic en el **Catálogo de aplicaciones en la nube**.

1. En el panel **Examinar por categoría**, selecciona **Almacenamiento en la nube**.

1. En la lista de aplicaciones, observa la **Puntuación de riesgo** junto al nombre de la aplicación.  

1. Abre otra pestaña del explorador y ve a **Dropbox.com**.

1. Podrás acceder a este sitio web.


#### Tarea 2: restringir aplicaciones en Defender for Cloud Apps

1. Vuelve al mosaico **Aplicaciones descubiertas** y selecciona **Etiquetar como no autorizada** para Dropbox.  **Nota**: esto se encuentra junto a la marca de verificación señalada.

1. Haga clic en **Guardar**

1. Este proceso te permite bloquear aplicaciones que no están autorizadas dentro de la directiva de tu empresa, lo que limita Shadow IT dentro de tu organización.

**Nota**: hay un tiempo de espera entre que se desautoriza una aplicación y se bloquea.

Una vez que se ha bloqueado la aplicación, no se podrá acceder a ella a través del explorador, el explorador InPrivate o la descarga desde la tienda en un cliente que esté incorporado a MDE (Microsoft Defender for Endpoint) integrado con Microsoft Defender for Cloud Apps.



