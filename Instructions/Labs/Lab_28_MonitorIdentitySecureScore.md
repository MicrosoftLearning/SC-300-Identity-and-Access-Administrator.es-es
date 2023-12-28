---
lab:
  title: 28 - Supervisión y administración de la postura de seguridad con la puntuación de seguridad de la identidad
  learning path: '04'
  module: Module 04 - Plan and Implement and Identity Governance Strategy
---

# Laboratorio 28: supervisión y administración de la posición de seguridad con la puntuación de seguridad de la identidad

## Escenario del laboratorio

Azure AD Identity Protection ofrece detección y corrección automatizadas a riesgos basados en identidad y proporciona datos en el portal para investigar posibles riesgos. Azure AD Identity Protection también proporciona una puntuación de seguridad de la identidad para supervisar y mejorar la posición de seguridad de la identidad.  De la misma manera que Microsoft 365 Defender y Microsoft Defender for Cloud, la puntuación de seguridad de identidad proporciona acciones de mejora y recomendaciones que pueden mejorar la posición general de seguridad de la identidad en Azure Active Directory.  Este laboratorio explorará esta funcionalidad. 

#### Tiempo estimado: 15 minutos

### Ejercicio 1: uso de la puntuación de seguridad de la identidad para supervisar y administrar la posición de seguridad de identidad

#### Tarea 1: revisar la puntuación de seguridad de identidad y las acciones de mejora

1. Inicia sesión en  [https://portal.azure.com](https://portal.azure.com) como Administrador global.

2. Busca y selecciona **Azure AD Identity Protection**.

3. En el mosaico **Información general**, encontrarás **Puntuación de seguridad de identidad**.

4. Selecciona **Puntuación de seguridad de identidad**.  Esto te llevará al panel Puntuación de seguridad de identidad.

5. Desplázate hacia abajo para ver las **acciones de mejora**.

6. A diferencia de las acciones de mejora de Microsoft Defender for Cloud y Microsoft 365 Defender, estas acciones de mejora son específicas de la identidad.  Esto proporciona una lista más enfocada en las posibles acciones de la administración de la posición de seguridad.  Las acciones de mejora iniciadas desde esta lista también proporcionarán un impacto en la posición de seguridad general del inquilino. 

#### Tarea 2: ejecutar una acción de mejora

1. Para mejorar un área de la postura de seguridad de identidades, selecciona **Proteger a todos los usuarios con una directiva de riesgo de inicio de sesión**.

2. En el mosaico que se abre, desplázate hacia abajo y selecciona **Comenzar**.

3. Se abrirá una nueva pestaña para **Identity Protection | Directiva de riesgo de inicio de sesión**.

4. Selecciona **Todos los usuarios** en **Asignaciones**.

5. Selecciona **Medio y superior** en **Riesgo de inicio de sesión**.

6. Selecciona **Permitir** - **Requerir la autenticación multifactor** en **Controles**.

7. Cambia la **Aplicación de directivas** a **Habilitada** (si todavía no lo has hecho) y selecciona **Guardar**.

8. Has creado una directiva de riesgo de inicio de sesión que ahora debe aumentar tu puntuación de seguridad de identidad.  Esto tardará hasta 24 horas en afectar la puntuación de seguridad de identidad.

9. Revisa otras acciones de mejora y los pasos para crearlas y habilitarlas.
