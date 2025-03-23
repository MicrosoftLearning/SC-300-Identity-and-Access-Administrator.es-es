---
lab:
  title: '28: supervisar y administrar la posición de seguridad con la puntuación de seguridad de la identidad'
  learning path: '04'
  module: Module 04 - Plan and Implement and Identity Governance Strategy
---

# Laboratorio 28: supervisar y administrar la posición de seguridad con la puntuación de seguridad de la identidad

### Tipo de inicio de sesión = Administración de Microsoft 365

## Escenario del laboratorio

Microsoft Entra Identity Protection proporciona detección y corrección automatizadas a riesgos basados en identidades y facilita datos en el portal para investigar posibles riesgos. Microsoft Entra Identity Protection también proporciona una puntuación de seguridad de identidad para supervisar y mejorar la posición de seguridad de la identidad.  De la misma manera que Microsoft Defender XDR y Microsoft Defender for Cloud, la puntuación de seguridad de identidad proporciona acciones de mejora y recomendaciones que pueden mejorar la posición general de seguridad para la identidad en Microsoft Entra ID.  Este laboratorio explorará esta funcionalidad. 

**Nota:** Dado que este laboratorio se ejecuta en un nuevo entorno de inquilino creado, probablemente obtendrás una puntuación de seguridad de la identidad del 30 % o menos.  Los datos viables tardan aproximadamente 24 horas en escribir el cálculo para proporcionarte una puntuación válida.

#### Tiempo estimado: 15 minutos

### Ejercicio 1: usar la puntuación de seguridad de identidad para supervisar y administrar la posición de seguridad de identidad

#### Tarea 1: Revisión de la puntuación de seguridad de identidad y acciones de mejora

1. Inicia sesión en  [https://entra.microsoft.com](https://entra.microsoft.com) como Administrador global.

2. Abre el menú **Protección** y selecciona **Puntuación de seguridad de la identidad**

3. En el icono **Información general**, encontrarás **Puntuación de seguridad de identidad**.

4. Selecciona **Puntuación de seguridad de identidad**.  Esto te llevará al panel Puntuación de seguridad de identidad.

5. Desplázate hacia abajo para ver las **Acciones de mejora**.

**Sugerencia de laboratorio**: a diferencia de las acciones de mejora de Microsoft Defender for Cloud y Microsoft Defender XDR, estas acciones de mejora son específicas de la identidad.  Esto proporciona una lista más enfocada en las posibles acciones de la administración de la posición de seguridad.  Las acciones de mejora iniciadas desde esta lista también proporcionarán un impacto en la posición de seguridad general del inquilino. 

#### Tarea 2: ejecutar una acción de mejora

1. Para mejorar una área de la posición de seguridad de identidad, selecciona **Habilitar la directiva de riesgo de inicio de sesión de Protección de Microsoft Entra ID**.

2. En el icono que se abre, desplázate hacia abajo y selecciona **Introducción**.

3. Se abrirá una nueva pestaña para **Identity Protection | Directiva de riesgo de inicio de sesión**.
 **Nota:** De forma predeterminada, se abrirá el botón Comenzar en Azure Portal. Puedes usar el portal o volver al centro de administración de Entra. Cualquiera funcionará.

6. En Asignaciones, el texto **Todos los usuarios**.

7. En Incluir, selecciona Todos los usuarios.

8. En Excluir, selecciona Usuarios y grupos y elige tu cuenta de **administrador de MOD**.

  - Microsoft recomienda excluir al menos una cuenta para evitar que se bloquee.

9. En Riesgo de acceso: selecciona el texto que dice **Baja y superior**.

10. Elige **Media y superior** y luego selecciona **Listo**.

10. En la sección **Controles** elige el texto que dice **Bloquear acceso**.

11. Selecciona **Conceder acceso: requerir autenticación multifactor**.

11. Selecciona Listo.

14. Confirma la configuración y establece la aplicación de directivas en **Habilitada**.

15. Seleccione **Guardar**.
