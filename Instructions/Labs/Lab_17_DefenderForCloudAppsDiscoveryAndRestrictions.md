---
lab:
  title: '17: detectar aplicaciones de Defender for Cloud Apps y aplicación de restricciones'
  learning path: '03'
  module: Module 03 - Implement Access Management for Apps
---

# Laboratorio 17: detectar aplicaciones de Defender for Cloud Apps y aplicar restricciones

## Escenario del laboratorio

Microsoft Defender for Cloud Apps utiliza registros del tráfico de red para identificar las aplicaciones a las que acceden los usuarios.  Los registros de tráfico de firewalls locales proporcionarán un informe de instantáneas sobre las aplicaciones más comunes y los usuarios que acceden a estas aplicaciones.El tráfico desde dispositivos administrados se insertará en el panel de información general de detección de Microsoft Defender for Cloud Apps.

#### Tiempo estimado: 10 minutos

### Ejercicio 1: detectar Defender for Cloud Apps

#### Tarea 1: detectar aplicaciones en Defender for Cloud Apps

1. Inicia sesión en [https://security.microsoft.com](https://security.microsoft.com)  con una cuenta de administrador global.

1. En el menú izquierdo, desplázate hasta el título denominado **Cloud Apps** y haz clic en **catálogo de aplicaciones en la nube**.

1. En el panel **Examinar por categoría**, selecciona ** Almacenamiento en la nube**.

1. En la lista de aplicaciones, anota la **puntuación de riesgo** junto al nombre de la aplicación.  

1. Abre otra pestaña del explorador y ve a **www.dropbox.com**.

1. Podrás acceder a este sitio web.


#### Tarea 2: restringir aplicaciones en Defender for Cloud Apps

1. Vuelve al icono **Aplicaciones detectadas** y selecciona la **Etiqueta como no autorizada** para Dropbox.  **Nota**: Esto se encuentra junto a la marca de verificación con un círculo.

1. Haga clic en **Guardar**

1. Este proceso te permite bloquear aplicaciones que no están autorizadas en la directiva de tu empresa, lo que limita Shadow IT en tu organización.

**Nota**: Hay un retraso entre no autorizar una aplicación y esa aplicación que se está bloqueando.

Una vez que la aplicación se ha bloqueado como no autorizada, la aplicación no será accesible a través del explorador, el explorador privado ni la descarga en un cliente que esté incorporado a MDE (Microsoft Defender para punto de conexión) integrado con Microsoft Defender for Cloud Apps.



