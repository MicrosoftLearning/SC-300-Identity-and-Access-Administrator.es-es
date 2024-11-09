---
lab:
  title: '28: supervisar y administrar la posición de seguridad con la puntuación de seguridad de la identidad'
  learning path: '04'
  module: Module 04 - Plan and Implement and Identity Governance Strategy
---

# Laboratorio 28: supervisar y administrar la posición de seguridad con la puntuación de seguridad de la identidad

## Escenario del laboratorio

Microsoft Entra Identity Protection proporciona detección y corrección automatizadas a riesgos basados en identidades y facilita datos en el portal para investigar posibles riesgos. Microsoft Entra Identity Protection también proporciona una puntuación de seguridad de identidad para supervisar y mejorar la posición de seguridad de la identidad.  De la misma manera que Microsoft Defender XDR y Microsoft Defender for Cloud, la puntuación de seguridad de identidad proporciona acciones de mejora y recomendaciones que pueden mejorar la posición general de seguridad para la identidad en Microsoft Entra ID.  Este laboratorio explorará esta funcionalidad. 

#### Tiempo estimado: 15 minutos

### Ejercicio 1: usar la puntuación de seguridad de identidad para supervisar y administrar la posición de seguridad de identidad

#### Tarea 1: Revisión de la puntuación de seguridad de identidad y acciones de mejora

1. Inicia sesión en  [https://entra.microsoft.com](https://entra.microsoft.com) como Administrador global.

2. Abre el menú **Protección** y selecciona **Puntuación de seguridad de la identidad**

3. En el icono **Información general**, encontrarás **Puntuación de seguridad de identidad**.

4. Selecciona **Puntuación de seguridad de identidad**.  Esto te llevará al panel Puntuación de seguridad de identidad.

5. Desplázate hacia abajo para ver las **Acciones de mejora**.

6. A diferencia de las acciones de mejora de Microsoft Defender for Cloud y Microsoft Defender XDR, estas acciones de mejora son específicas de la identidad.  Esto proporciona una lista más enfocada en las posibles acciones de la administración de la posición de seguridad.  Las acciones de mejora iniciadas desde esta lista también proporcionarán un impacto en la posición de seguridad general del inquilino. 

#### Tarea 2: ejecutar una acción de mejora

1. Para mejorar una área de la posición de seguridad de identidad, selecciona **Habilitar directivas de riesgo de inicio de sesión de protección de identidades de Microsoft Entra ID**.

2. En el icono que se abre, desplázate hacia abajo y selecciona **Introducción**.

3. Se abrirá una nueva pestaña para el **Acceso condicional**.
 **Nota:** De forma predeterminada, se abrirá el botón Comenzar en Azure Portal. Puedes usar el portal o volver al centro de administración de Entra. Cualquiera funcionará.

4. Selecciona **+ Nueva directiva**.

5. Asigna un nombre a la directiva. Se recomienda que las organizaciones creen un estándar significativo para los nombres de sus directivas.

6. En Asignaciones, selecciona Identidades de carga de trabajo o usarios.

7. En Incluir, seleccione Todos los usuarios.

8. En Excluir, seleccione Usuarios y grupos y elija las cuentas que deben mantener la capacidad para usar la autenticación heredada. Microsoft recomienda excluir al menos una cuenta para evitar que se bloquee.

9. En Recursos de destino> Aplicaciones en la nube> Incluir, selecciona Todas las aplicaciones en la nube.

10. En Condiciones > Aplicaciones cliente, establece Configurar en Sí.
 - Active solo las casillas Clientes de Exchange ActiveSync y Otros clientes.

11. Seleccione Listo.

12. En Controles de acceso > Conceder, selecciona Bloquear acceso.

13. Elija Seleccionar.

14. Confirme la configuración y establezca Habilitar directiva en Solo informe.

15. Selecciona Crear para crear la directiva.
