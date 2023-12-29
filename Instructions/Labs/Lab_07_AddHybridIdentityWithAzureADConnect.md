---
lab:
  title: '07: opcional --- agregar identidad híbrida con Microsoft Entra Connect'
  learning path: '01'
  module: Module 01 - Implement an identity management solution
---

# Laboratorio 07: OPCIONAL --- agregar identidad híbrida con Microsoft Entra Connect



# Este laboratorio no funciona actualmente.  Debido a un cambio de licencia en Microsoft Entra ID, el laboratorio falla.  Actualmente estamos solucionando y actualizando el laboratorio, y deberíamos tenerlo en línea en un plazo de una semana.  Muévete al siguiente laboratorio.




**Nota:** este laboratorio requiere un pase para Azure. Consulta el laboratorio 00 para obtener instrucciones.

**Nota 2**: Este laboratorio se denomina Opcional.  Tarda al menos 1 hora en completarse y requiere que se detallen los pasos del laboratorio.  No dudes en computarlo cuando el tiempo lo permita.  Si tu empresa ya ha configurado su configuración híbrida o no tienes previsto usar Microsoft Entra Connect, salta este laboratorio.

## Escenario del laboratorio

Tu empresa tiene servicios de dominio de Active Directory locales.  Les gustaría seguir usando Active Directory local como su solución de administración de identidades y acceso, pero también necesitan que los usuarios accedan a las aplicaciones en la nube con el mismo nombre de usuario y contraseña.

#### Tiempo estimado: 60 minutos

### Ejercicio 1: configuración de la infraestructura local

#### Tarea 1: crear la infraestructura de Active Directory local

1. Se puede acceder a la plantilla de implementación en este vínculo: [Guía del laboratorio de pruebas local](https://github.com/maxskunkworks/TLG/tree/master/tlg-base-config_3-vm).

    **Nota para los estudiantes y MCT**: la implementación de esta plantilla puede tardar entre 30 y 60 minutos, por lo que debes estar listo para tomar un descanso en este paso o ejecutar la implementación antes de una sección de clase del curso.

    **Nota para los proveedores de laboratorio**: si es posible, sería útil que los alumnos completaran e implementaran como parte de la configuración del entorno de laboratorio.

2. En la página **TLG (Guía del laboratorio de pruebas): configuración base de 3 VM  (v1.0),** selecciona **Implementar en Azure**.

   **Nota:** La configuración base de 3 VM aprovisiona un controlador de dominio de Active Directory de Windows Server 2016 denominado DC1 con el nombre de dominio que especifiques y un servidor miembro de dominio denominado APP1 que ejecuta Windows Server 2016. También ofrece una opción para aprovisionar una máquina virtual cliente que ejecuta Windows 10, pero no la usaremos en nuestro laboratorio (básicamente debido a los requisitos de licencia aplicables al ejecutar máquinas virtuales de Windows 10 en Azure). El servidor miembro del dominio (APP1) ha instalado automáticamente .NET 4.5 e IIS.  
   
   **Nota:** La máquina virtual necesaria para este laboratorio es **DC1**.  Si usas un Pase para Azure, hay una limitación de 2 máquinas virtuales, por lo que es posible que se produzca un error en la máquina virtual cliente.  No la necesitaremos en este laboratorio.

3. En la página **Implementación personalizada**, especifica la siguiente configuración y luego selecciona **Revisar y crear** y luego **Crear**.

   -   Suscripción: el nombre de la suscripción de Azure de destino donde quieres aprovisionar las máquinas virtuales de Azure del entorno de laboratorio.
   -   Grupo de recursos: (Crear nuevo) **hybrididentity-RG**
   -   Ubicación: el nombre de la región de Azure que hospedará las máquinas virtuales de Azure del entorno de laboratorio.
   -   Nombre de configuración: **TlgBaseConfig-01**
   -   Nombre del dominio: **corp.contoso.com**
   -   Sistema operativo del servidor: **2016-Datacenter**
   -   Nombre de usuario administrador: **demouser**
   -   Contraseña del administrador: **escribe una contraseña segura que recordarás.**
   -   Implementación de una máquina virtual cliente: **No**
   -   URI de VHD de cliente: **deja en blanco**
   -   Tamaño de máquina virtual: **Standard_D2s_v3**
   
   **Nota:** Usa un tamaño de máquina virtual similar si la suscripción no admite el tamaño de la lista. La documentación está vinculada aquí: <https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes>.

   -   Prefijo de etiqueta DNS: **cualquier nombre DNS válido y único global (una cadena única formada por letras, dígitos y guiones, empezando por una letra y hasta 47 caracteres de longitud).**

   -   Ubicación de _artifactos: acepta el valor predeterminado. ****
   -   Token de Sas de ubicación de _artefactos: **deja este valor en blanco**

4. Seleccione **Revisar + crear**.

5. Seleccione **Crear** una vez que se pase la validación.
    
6. Espere a que la implementación se complete. Esto puede tardar unos 60 minutos.


### Tarea 2: configurar el entorno de laboratorio de las máquinas virtuales de Azure

1. En la ventana del explorador que muestra Azure Portal, ve a la máquina virtual de **DC1** Azure y conéctate a ella a través de Escritorio remoto. Cuando se te solicite, inicia sesión usando las siguientes credenciales:

   -   Nombre de usuario: **demouser**
   -   Contraseña: **usa la contraseña segura que has creado en la tarea 1**

2.  En la sesión de Escritorio remoto a **DC1**, inicia **Windows PowerShell ISE** y luego abre el panel Script.  A continuación, agrega el siguiente script al panel de scripts y ejecútalo para deshabilitar la configuración de seguridad mejorada de Internet Explorer y control de acceso de usuario en máquinas virtuales de Azure **DC1** y **APP1**:

    ```pwsh

    $vmNames = @('dc1','app1')
    Invoke-Command -ComputerName $vmNames {Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A7-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0}
    Invoke-Command -ComputerName $vmNames {Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A8-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0}
    Invoke-Command -ComputerName $vmNames {Set-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" -Name "ConsentPromptBehaviorAdmin" -Value 00000000}
    ```

    **Nota:** Para ejecutar varios scripts de PowerShell en el mismo archivo, puedes resaltar un script específico y seleccionar **Ejecutar selección** junto al botón de reproducción verde. 

3.  En la ventana **Windows PowerShell ISE**, agrega el siguiente script al panel de scripts y ejecútalo para instalar Herramientas de administración remota del servidor en máquinas virtuales de Azure *DC1* y **APP1**:

    ```pwsh

    $vmNames = @('dc1','app1')
    Invoke-Command -ComputerName $vmNames {Install-WindowsFeature RSAT -IncludeAllSubFeature} 
    ```

4.  En la ventana **Windows PowerShell ISE**, agrega el siguiente script al panel de scripts y ejecútalo para habilitar TLS 1.2 en las máquinas virtuales de Azure *DC1* y **APP1**:

    ```pwsh

    $vmNames = @('dc1','app1')
    Invoke-Command -ComputerName $vmNames {New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -Force}
    Invoke-Command -ComputerName $vmNames {New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -Force}
    Invoke-Command -ComputerName $vmNames {New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'Enabled' -value 1 –PropertyType DWORD}
    Invoke-Command -ComputerName $vmNames {New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'DisabledByDefault' -value 0 –PropertyType DWORD}
    Invoke-Command -ComputerName $vmNames {New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'Enabled' -value 1 –PropertyType DWORD}
    Invoke-Command -ComputerName $vmNames {New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'DisabledByDefault' -value 0 –PropertyType DWORD}
    Invoke-Command -ComputerName $vmNames {New-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\.NETFramework\v4.0.30319' -name 'SchUseStrongCrypto' -value 1 –PropertyType DWORD}
    ```

5.  En la ventana **Windows PowerShell ISE**, agrega el siguiente script al panel de scripts y ejecútalo para configurar la autenticación integrada de Windows en el sitio web predeterminado hospedado en la máquina virtual de Azure **APP1**:

    ```pwsh

    $vmNames = @('app1')
    Invoke-Command -ComputerName $vmNames {Enable-WindowsOptionalFeature -Online -FeatureName IIS-WindowsAuthentication}
    Invoke-Command -ComputerName $vmNames {Set-WebConfigurationProperty -Filter "/system.webServer/security/authentication/anonymousAuthentication" -Name Enabled -Value False -PSPath IIS:\ -Location "Default Web Site"}
    Invoke-Command -ComputerName $vmNames {Set-WebConfigurationProperty -Filter "/system.webServer/security/authentication/windowsAuthentication" -Name Enabled -Value True -PSPath IIS:\ -Location "Default Web Site"}
    ```

### Tarea 3: reiniciar las máquinas virtuales de Azure

1. En la ventana de **Windows PowerShell ISE**, en el panel de la consola, ejecute lo siguiente para reiniciar **APP1**:

    ```pwsh

    Restart-Computer -ComputerName 'APP1'
    ```

2. En la ventana de **Windows PowerShell ISE**, en el panel de consola, ejecuta lo siguiente para reiniciar **DC1**:

    ```pwsh
    Restart-Computer -ComputerName 'DC1'
    ```

### Tarea 5: configurar contoso.local de Active Directory

1. Conéctate de nuevo a la máquina virtual de Azure **DC1** a través del Escritorio remoto. Cuando se te solicite, inicia sesión usando las siguientes credenciales:

    -   Nombre de usuario: **demouser**

    -   Contraseña: **demo\@pass123**
       - **Se recomienda encarecidamente escribir una contraseña segura que puedas recordar.**

2.  En la sesión de Escritorio remoto a **DC1**, inicia Internet Explorer y ve al vínculo siguiente.

    ```
    https://github.com/microsoft/MCW-Hybrid-identity/tree/main/Archive/Hands-on%20lab/studentfiles
    ```

3. En la página **Crear usuarios o grupos para el entorno de demostración o prueba de Active Directory**, selecciona el vínculo **CreateDemoUsers.ps1**, acepta los términos de licencia y guarda el script correspondiente en el sistema de archivos local.

4. En la página **Crear usuarios o grupos para el entorno de demostración o prueba de Active Directory**, selecciona el vínculo **CreateDemoUsers.csv (directamente encima de la sección código de PowerShell) y guarda el archivo CSV** correspondiente en la misma ubicación que el archivo **CreateDemoUsers.ps1** .

5. En la sesión de Escritorio remoto a **DC1**, inicia el Explorador de archivos, ve a la carpeta donde has descargado ambos archivos, selecciona el archivo **CreateDemoUsers.ps1**, selecciona **Propiedades**, en el cuadro de diálogo **Propiedades CrearDemoUsers.ps1**, activa la casilla **Desbloquear** y selecciona **Aceptar**.

6. En la ventana del Explorador de archivos, haz clic con el botón derecho en el archivo **CreateDemoUsers.ps1** de nuevo y selecciona **Editar**. 

7. En la ventana **Administrador: Windows PowerShell ISE**, cambia la línea **148** de:

    ```pwsh
    $UserCount = 1000 #Up to 2500 can be created
    ```

   to
    ```pwsh
    $UserCount = 2500 #Up to 2500 can be created
    ```

8. En la ventana de **Windows PowerShell ISE**, guarda el cambio y ejecuta el script **CreateDemoUsers.ps1** para crear una jerarquía de unidades organizativas del entorno de laboratorio y rellenarla con cuentas de usuario de prueba. 

9.  En la ventana de **Windows PowerShell ISE**, agrega el siguiente script al panel de scripts y ejecútalo para modificar la configuración de las cuentas de usuario de AD que usarás en este laboratorio:

    ```pwsh

    $adUser1 = Get-ADUser -Filter {samAccountName -eq "AGAyers"}
    $adUser1groups = $adUser1 | Get-ADPrincipalGroupMembership 
    $adUser1groups | foreach { if ($_.name -ne 'Domain Users') {Remove-ADPrincipalGroupMembership -MemberOf $_.name -Identity $adUser1.DistinguishedName} }
    Add-ADPrincipalGroupMembership -MemberOf 'Engineering' -Identity $adUser1.DistinguishedName
    Move-ADObject -Identity $adUser1.DistinguishedName -TargetPath 'OU=NJ,OU=US,OU=Users,OU=Demo Accounts,DC=corp,DC=contoso,DC=com'

    Set-ADAccountPassword -Identity 'CN=Ayers\, Ann,OU=NJ,OU=US,OU=Users,OU=Demo Accounts,DC=corp,DC=contoso,DC=com' -Reset -NewPassword (ConvertTo-SecureString -AsPlainText "demo@pass123" -Force)

    $adUser2 = Get-ADUser -Filter {samAccountName -eq "TFBell"}
    $adUser2groups = $adUser2 | Get-ADPrincipalGroupMembership 
    $adUser2groups | foreach { if ($_.name -ne 'Domain Users') {Remove-ADPrincipalGroupMembership -MemberOf $_.name -Identity $adUser2.DistinguishedName} }
    Add-ADPrincipalGroupMembership -MemberOf 'Engineering' -Identity $adUser2.DistinguishedName
    Move-ADObject -Identity $adUser2.DistinguishedName -TargetPath 'OU=VT,OU=US,OU=Users,OU=Demo Accounts,DC=corp,DC=contoso,DC=com'

    Set-ADAccountPassword -Identity 'CN=Bell\, Teresa,OU=VT,OU=US,OU=Users,OU=Demo Accounts,DC=corp,DC=contoso,DC=com' -Reset -NewPassword (ConvertTo-SecureString -AsPlainText "demo@pass123" -Force)
    Get-ADGroup -Identity 'Domain Admins' | Add-ADGroupMember -Members 'CN=Ayers\, Ann,OU=NJ,OU=US,OU=Users,OU=Demo Accounts,DC=corp,DC=contoso,DC=com'
    Get-ADGroup -Identity 'Enterprise Admins' | Add-ADGroupMember -Members 'CN=Ayers\, Ann,OU=NJ,OU=US,OU=Users,OU=Demo Accounts,DC=corp,DC=contoso,DC=com'
    ```

10. En la ventana de **Windows PowerShell ISE**, agrega el siguiente script al panel de scripts y ejecútalo para crear unidades organizativas adicionales denominadas **Servers** y **Clients** y mover la cuenta de equipo ** APP1** a la primera de ellas:

    ```pwsh

    New-ADOrganizationalUnit -Name 'Servers' -Path 'OU=Demo Accounts,DC=corp,DC=contoso,DC=com'
    New-ADOrganizationalUnit -Name 'Clients' -Path 'OU=Demo Accounts,DC=corp,DC=contoso,DC=com'

    Move-ADObject -Identity 'CN=APP1,CN=Computers,DC=corp,DC=contoso,DC=com' -TargetPath 'OU=Servers,OU=Demo Accounts,DC=corp,DC=contoso,DC=com'
    ```

11. Cierra sesión de **DC1**.

## Ejercicio 2: integrar un bosque de Active Directory con un inquilino de Azure Active Directory

### Tarea 1: crear un inquilino de Azure Active Directory y habilitar una prueba de EMS E5

En esta tarea, crearás un inquilino de Azure Active Directory con la siguiente configuración: 

-   Nombre de la organización: **Contoso**

-   Nombre de dominio inicial: cualquier nombre de dominio válido y único.

-   País o región: **Estados Unidos**

1. En el equipo de laboratorio, inicia una nueva ventana del explorador web y ve a Azure Portal en <https://portal.azure.com> si todavía no lo has hecho.

2. Cuando se te solicite, inicia sesión en la suscripción de Azure en la que has implementado recursos en los ejercicios del laboratorio anterior a la práctica.

3. En el equipo del laboratorio, en Azure Portal selecciona **+ Crear un recurso**.

4. En la página **Nuevo**, en el cuadro de texto **Buscar en Marketplace**, escribe **Azure Active Directory** y, en la lista de resultados, selecciona **Azure Active Directory**.

5. En la página **Azure Active Directory**, selecciona **Crear**.

6. En la página **Crear directorio**, escribe los siguientes valores y selecciona **Crear**:

Pestaña Básico:
    -   Selecciona el tipo de inquilino: elige **Azure Active Directory**

Pestaña Configuración:
    -   Nombre de la organización: **Contoso**

    -   Nombre de dominio inicial: cualquier nombre de dominio válido y único.

    -   País o región: **Estados Unidos**

7. Una vez creado, abre **Azure Active Directory**.

8. En la página Información general, selecciona **Administrar inquilinos**.

9. Coloca una actualización en el directorio recién creado.

10. En la parte superior de la página, elige **Switch**.

    >**Nota**: Puede tardar unos minutos en mostrarse en todas partes.

11. En la página **Contoso - Introducción**, selecciona **Usuarios**.

12. Observa que solo tienes un único usuario ExternalAzureAD en este nuevo inquilino.

### Tarea 2: Creación y configuración de un usuario de Azure AD para administrar este directorio

1. En el equipo del laboratorio, en Azure Portal, vuelve a la página **Contoso - Introducción**.

2. En la página **Contoso - Introducción**, selecciona **Usuarios** en **Administrar** en el panel de navegación izquierdo.

3. En la hoja **Usuarios - Todos los usuarios**, haz clic en la entrada que representa tu cuenta de usuario.

4. En la página **Perfil** de tu cuenta de usuario selecciona **Editar**.

5. En la sección **Configuración**, en la lista desplegable **Ubicación de uso** selecciona la entrada **Estados Unidos** y selecciona **Guardar**.

#### Creación del nuevo administrador

6. En el panel **Nuevo usuario**, asegúrate de que la opción **Crear usuario** está seleccionada, especifica la siguiente configuración y selecciona **Crear**:

    - Nombre de usuario: **john.doe *@yourNombre de dominio de inquilino de Azure AD*** donde ***tu nombre de dominio del inquilino de Azure AD*** es el nombre de dominio que has especificado al crear el inquilino de Contoso Azure AD.

    - Nombre: **john.doe**

    - Nombre: **John**

    - Apellido: **Doe**
    
    - Contraseña: **Generar automáticamente la contraseña**
    
    - Mostrar contraseña: **Habilitado**, asegúrate de copiar la contraseña.

    - Grupos: **0 grupos seleccionados**
    
    - Roles: **administrador global**
    
    - Bloquear inicio de sesión: **No**
    
    - Ubicación de uso: **Estados Unidos**
    
    - Puesto: **deja en blanco**
    
    - Departamento: **deja en blanco**

    > **Nota**: Cos valores **Nombre de usuario** y **Contraseña** en Bloc de notas. Los necesitará más adelante en este laboratorio.


### Tarea 5: configurar el sufijo DNS en el bosque de Contoso Active Directory

En esta tarea, configurarás el sufijo DNS del bosque de Contoso Active Directory para que coincida con el nombre de dominio personalizado de Azure AD recién comprobado.

1. En el equipo del laboratorio, en Azure Portal, comprueba que has iniciado sesión en el inquilino de Azure AD asociado a la suscripción de Azure en la que has implementado recursos en los ejercicios del laboratorio práctico previos (**Directorio predeterminado**). Si no es así, selecciona el icono **Directorio y suscripción** de la barra de herramientas de Azure Portal (a la derecha del icono de **Cloud Shell**) para cambiar a ese inquilino de Azure AD. 

2. En Azure Portal, ve a la página de la máquina virtual **DC1**.

3. En la página de la máquina virtual **DC1**, conéctate a ** DC1 ** a través del Escritorio remoto. Cuando se te pida que inicies sesión, usa el nombre **demouser** y la **contraseña de paso \@pass123** de la demo. 

4. Dentro de la sesión de Escritorio remoto a **DC1**, en la ventana de **Administrador del servidor**, inicie la consola **Dominios y confianzas de Active Directory** en **Herramientas**. 

5. En la consola **Dominios y confianzas de Active Directory**, selecciona **Dominios y confianzas de Active Directory [DC1.corp.contoso.com]** a la izquierda y selecciona **Propiedades**.

6. En la pestaña **Sufijos UPN** de la ventana **Dominios y confianzas de Active Directory [DC1.corp.contoso.com],** en el cuadro de diálogo **Sufijos UPN alternativos**, escribe el nombre del dominio personalizado que has comprobado en la tarea anterior, selecciona **Agregar** y luego selecciona **Aceptar**.

7. En la sesión de Escritorio remoto a **DC1**, en la ventana **Administrador del servidor**, inicia la consola **Usuarios y equipos de Active Directory** en **Herramientas**. 

8. En la consola **Usuarios y equipos de Active Directory**, expande **corp.contoso.com** a la izquierda y examina la jerarquía de unidades organizativas del dominio y la pertenencia al grupo de los grupos de dominios. 

9.  En la sesión de Escritorio remoto a **DC1**, inicia Windows PowerShell ISE y, en el panel Script, ejecuta lo siguiente para reemplazar el sufijo UPN de todos los usuarios que son miembros del grupo **Ingeniería** por el que coincida con el nombre de dominio comprobado personalizado del inquilino de Azure AD de Contoso (reemplaza el marcador de posición `<custom_domain_name>` por el nombre de dominio comprobado personalizado que has asignado al inquilino de Azure AD de Contoso). 

    ```pwsh
    $domainName = '<custom_domain_name>'
    $users = Get-ADGroupMember -Identity 'Engineering' -Recursive | Where-Object {$_.objectClass -eq 'user'}

    foreach ($user in $users) {
        $user = Get-ADUser -Identity $User.SamAccountName
        $userName = $user.UserPrincipalName.Split('@')[0] 
        $upn = $userName + "@" + $domainName 
        $user | Set-ADUser -UserPrincipalName $upn
    }
    ```

### Tarea 6: Instalación de Microsoft Entra Connect

En esta tarea, instalarás Microsoft Entra Connect.

1. En la sesión de Escritorio remoto a **DC1**, en Administrador del servidor, selecciona **Servidor local** y asegúrate de que **la configuración de seguridad mejorada de IE** esté deshabilitada. Si no es así, selecciona el vínculo **Activado** junto a **Configuración de seguridad mejorada de IE**, establece la configuración de los **Administradores** en **Desactivado** y selecciona **Aceptar**.

2. En la sesión de Escritorio remoto a **DC1**, abre la ventana de **Windows PowerShell ISE** y ejecuta este comando para instalar el explorador Chrome.

    ```pwsh
    $LocalTempDir = $env:TEMP; $ChromeInstaller = "ChromeInstaller.exe"; (new-object System.Net.WebClient).DownloadFile('http://dl.google.com/chrome/install/375.126/chrome_installer.exe', "$LocalTempDir\$ChromeInstaller"); & "$LocalTempDir\$ChromeInstaller" /silent /install; $Process2Monitor = "ChromeInstaller"; Do { $ProcessesFound = Get-Process | ?{$Process2Monitor -contains $_.Name} | Select-Object -ExpandProperty Name; If ($ProcessesFound) { "Still running: $($ProcessesFound -join ', ')" | Write-Host; Start-Sleep -Seconds 2 } else { rm "$LocalTempDir\$ChromeInstaller" -ErrorAction SilentlyContinue -Verbose } } Until (!$ProcessesFound)
    ```

2. En la sesión de Escritorio remoto a **DC1**, inicia el explorador Chrome y ve a Azure Portal en <https://portal.azure.com>.

3. Cuando se te pida que inicies sesión, escribe las credenciales de la cuenta de usuario de Microsoft Entra **john.doe** que has copiado en el Bloc de notas anterior en este ejercicio.

4. Cuando se te solicite, escribe la contraseña de la cuenta de usuario **john.doe**. 
  
    > **Nota**: Si recibes el mensaje **Hemos visto esta contraseña demasiadas veces antes. Elija algo más difícil de adivinar**, deberás modificar la contraseña hasta que sea lo suficientemente única para aceptarse.

5. Si se te pregunta si quieres **mantener la sesión iniciada** seleccione **No**. Se te redirigirá a la interfaz de Azure Portal. 

6. Si se presenta el cuadro de diálogo **Bienvenido a Microsoft Azure**, selecciona **Quizá más adelante**. 

7. En Azure Portal, busca **Microsoft Entra Connect**.

8. En los resultados de la búsqueda, selecciona **Microsoft Entra Connect**.

9.  En la página **Microsoft Entra Connect**, selecciona el vínculo **Descargar Microsoft Entra Connect**.  A continuación, elige **Connect Sync** en el menú.

10. En la página de descarga de **Microsoft Azure Active Directory Connect v2**, selecciona **Descargar**.

11. Si se te pide que ejecutes o guardes **AzureADConnect.msi**, selecciona **Ejecutar**. Esto descargará el archivo e iniciará automáticamente el asistente **Microsoft Azure Active Directory Connect**. 

12. En la página **Bienvenido a Azure AD Connect** comprueba el cuadro **Acepto los términos de licencia y el aviso de privacidad** y selecciona **Continuar**.

13. En la página **Configuración rápida**, selecciona el botón **Personalizar**.

14. En la página **Instalar componentes necesarios**, deje desactivadas todas las opciones de configuración opcionales y seleccione **Instalar**.

15. En la página **Inicio de sesión de usuario**, selecciona la opción **Autenticación transferida** y la casilla **Habilitar inicio de sesión único** y luego selecciona **Siguiente**.

16. En la página **Conectar a Azure AD**, inicia sesión con las credenciales de la cuenta **john.doe** y selecciona **Siguiente**.

17. En la página **Conectar sus directorios**, asegúrate de que la entrada ** corp.contoso.com** aparece en la lista desplegable **BOSQUE** y selecciona **Agregar directorio**. En la **Cuenta del bosque de AD**, asegúrate de que la opción **Crear una cuenta de AD** esté seleccionada, en el cuadro de diálogo **NOMBRE DE USUARIO DE ADMINISTRADOR DE EMPRESA**, escribe **CORP.CONTOSO.COM\\demouser**, en el cuadro de texto **CONTRASEÑA** , escribe **demo\@pass123** y selecciona **Aceptar**.


18. En la página **Conectar sus directorios**, selecciona **Siguiente**.

19. En la página **Configuración de inicio de sesión de Azure AD,**, asegúrate de que el nombre de dominio personalizado aparece como el sufijo **Sufijo UPN de Active Directory ** comprobado y que la entrada **userPrincipalName** aparece en la lista desplegable **NOMBRE PRINCIPAL DE USUARIO**. Ten en cuenta la advertencia que indica que **los usuarios no podrán iniciar sesión en Azure AD con credenciales locales si el sufijo UPN no coincide con un nombre de dominio comprobado**. Activa la casilla **Continuar sin relacionar todos los sufijos UPN con los dominios verificados** y selecciona **Siguiente**. 

    >**Nota**: Esto es de esperar, ya que algunos usuarios todavía están configurados con el sufijo de UPN **contoso.local**, que no es enrutable y no se puede configurar como un nombre de dominio personalizado verificado de un inquilino de Azure AD.

20. En la página **Filtrado de dominios y unidades organizativas** selecciona **Sincronizar los dominios y las unidades organizativas seleccionados** y luego asegúrate de que solo están seleccionadas las unidades organizativas **DemoAccounts** y todas sus unidades organizativas secundarias y selecciona **Siguiente**. 


21. En la página **Identificación de forma exclusiva de usuarios**, acepta la configuración predeterminada y selecciona **Siguiente**. 


22. En la página **Filtrar usuarios y dispositivos,** acepta la configuración predeterminada y selecciona **Siguiente**. 

23. En la página **Características opcionales**, acepta la configuración predeterminada y selecciona **Siguiente**.

24. En la página **Habilitar inicio de sesión único**, selecciona **Especificar credenciales**, en el cuadro de diálogo **Credenciales del bosque**, inicia sesión con el nombre de usuario **CORP\\demouser** y la contraseña **demo\@pass123** y selecciona **Siguiente**.


25. En la página **Listo para configurar**, asegúrate de que la casilla **inicie el proceso de sincronización cuando se complete la configuración** **NO** esté seleccionada y selecciona **Instalar**.


   > **Nota**: Configurarás el filtrado de nivel de atributo antes de habilitar el proceso de sincronización.

   > **Nota**: La instalación debería tardar aproximadamente 2 minutos.

26. En la página **Configuración completada**, seleccione **Salir**.


### Tarea 7: habilitar la papelera de reciclaje de Active Directory

En esta tarea, habilitarás la Papelera de reciclaje en el dominio Active Directory de Contoso. 

1. En la sesión de Escritorio remoto a **DC1**, en el menú Herramientas de la consola de Administrador del servidor, inicia el **Centro de Administración de Active Directory**.


2. En la consola del **Centro Administración de Active Directory**, selecciona **corp (local)** a la izquierda y selecciona **Habilitar papelera de reciclaje**. Cuando se le pida confirmación, seleccione **Aceptar**.


3. Cuando se te pida que actualices el Centro de Administración de AD, selecciona **Aceptar**.

   > **Nota**: Para obtener información sobre las ventajas de la papelera de reciclaje en escenarios híbridos, consulta <https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-sync-recycle-bin>

### Tarea 8: configurar el filtrado de nivel de atributo de Azure AD Connect

En esta tarea, configurarás el filtrado de nivel de atributo de Azure AD Connect que limitará la sincronización de cuentas de usuario con el sufijo UPN que coincida con el nombre de dominio personalizado del inquilino de Azure AD de destino.

   > **Nota**: La opción de filtrado positivo requiere dos reglas de sincronización. Una de ellas determina el ámbito correcto de los objetos que se van a sincronizar. La segunda regla de sincronización de comodín filtra todos los objetos que aún no han sido identificados como objetos que deben sincronizarse.

1. En la sesión de Escritorio remoto a **DC1**, inicia **el Editor de reglas de sincronización**en **Azure AD Connect** en el menú Inicio.


2. En la ventana Editor de reglas de sincronización, en la página **Ver y administrar las reglas de sincronización**, asegúrate de que **Entrada** aparezca en la lista desplegable **Dirección** y seleccione **Agregar nueva regla**. Se iniciará el asistente **Crear reglas de sincronización de entrada**.


3. En la página **Crear regla de sincronización de entrada - descripción**, especifica la siguiente configuración y selecciona **Siguiente**:

    - Nombre: **Custom In from AD - UPN Filter**

    - Descripción: **regla de entrada personalizada - incluye usuarios con UPN configurado para que coincidan con el dominio personalizado de Azure AD.**

    - Sistema conectado: **corp.contoso.com**

    - Tipo de objeto de sistema conectado: **usuario**

    - Tipo de objeto de metaverso: **persona**

    - Tipo de vínculo: **unirse**

    - Precedencia: **50**

    - Etiqueta: **dejar vacío**

    - Habilitar sincronización de contraseñas: **dejar vacío**

    - Deshabilitado: **dejar vacío**


4. En la página ** Crear filtro de ámbito de entrada**, selecciona **Agregar grupo**, selecciona **Agregar cláusula** especifica lo siguiente y selecciona **Siguiente**:

    - Atributo: **userPrincipalName**

    - Operador: **ENDSWITH**

    - Valor: **\@\<your custom domain name>**

5. En la página **Unir reglas**, selecciona **Siguiente**.

6. En la página **Transformaciones**, selecciona **Agregar transformación** especifica lo siguiente y selecciona **Agregar**:

    - Tipo de flujo: **Constante**

    - Atributo de destino: **cloudFiltered**

    - Origen: **False**

7. Cuando se presenta un cuadro de diálogo **Advertencia** que muestra el mensaje que indica que **se ejecutará una importación y sincronización completas en “corp.contoso.com” durante el siguiente ciclo de sincronización**, selecciona **Aceptar**.

   > **Nota**: Esto debería devolverte a la vista y administrar la interfaz de reglas de sincronización, con la nueva regla que aparece en la parte superior de la lista de reglas. 

8. De nuevo en la ventana **Editor de reglas de sincronización**, en la página **Ver y administrar las reglas de sincronización**, asegúrate de que **Entrante** aparece en la lista desplegable **Dirección** y selecciona **Agregar nueva regla** de nuevo. Se iniciará el asistente **Crear reglas de sincronización de entrada**.

9. En la página **Descripción**, especifica los elementos siguientes y selecciona **Siguiente**:

    - Nombre: **Custom In from AD: filtro comodín**

    - Descripción: **regla de entrada personalizada: excluye a todos los usuarios con UPN no configurado para que coincidan con el dominio personalizado de Azure AD.**

    - Sistema conectado: **corp.contoso.com**

    - Tipo de objeto de sistema conectado: **usuario**

    - Tipo de objeto de metaverso: **persona**

    - Tipo de vínculo: **unirse**

    - Precedencia: **51**

    - Etiqueta: **dejar vacío**

    - Habilitar sincronización de contraseñas: **dejar vacío**

    - Deshabilitado: **dejar vacío**


10. En la pantalla **Filtro de ámbito**, selecciona **Siguiente**.

11. En la página **Unir reglas**, selecciona **Siguiente**.

12. En la página **Transformaciones**, selecciona **Agregar transformación** especifica lo siguiente y selecciona **Agregar**:

    - Tipo de flujo: **Constante**

    - Atributo de destino: **cloudFiltered**

    - Origen: **True**

13. Cuando se presente un cuadro de diálogo de **Advertencia** que muestra un mensaje que indica que **se ejecutará una importación y sincronización completas en “corp.contoso.com” durante el siguiente ciclo de sincronización**, selecciona **Aceptar**.

    >**Nota**: Esto debería traerte de vuelta a la interfaz **Ver y administrar las reglas de sincronización**, con las nuevas reglas enumeradas en la parte superior de la lista de reglas. 

### Tarea 9: iniciar y comprobar la sincronización de directorios

1. En la sesión de Escritorio remoto a **DC1**, haz doble clic en el acceso directo de escritorio de **Azure AD Connect**.

2. En la página **Bienvenido a Azure AD Connect**, haz clic en **Configurar**. 

3. En la página **Tareas adicionales**, selecciona **Personalizar las opciones de sincronización** y selecciona **Siguiente**.

4. En la página **Conectar a Azure AD**, inicia sesión con las credenciales de la cuenta **john.doe** y selecciona **Siguiente**.

5. En la página **Conectar sus directorios**, haga clic en **Siguiente**.

6. En la pantalla **Filtrado de dominios y unidades organizativas**, seleccione **Siguiente**. 

7. En la página **Características opcionales**, acepta la configuración predeterminada y selecciona **Siguiente**.

8. En la página **Habilitar inicio de sesión único**, selecciona **Siguiente**.

9. En la página **Listo para configurar**, selecciona la casilla **Iniciar el proceso de sincronización cuando finalice la configuración** y selecciona **Configurar**.

10. En la página **Configuración completada**, seleccione **Salir**.

11. En la sesión de Escritorio remoto para **DC1**, en la ventana de Edge que muestra Azure Portal, ve a la página **Usuarios - Todos los usuarios** del inquilino de Contoso Azure AD.

12. En la página **Usuarios - todos los usuarios**, ten en cuenta que la lista de objetos de usuario incluye todas las cuentas de usuario con el sufijo UPN que coincide con el nombre de dominio personalizado del inquilino de Azure AD. Es posible que tengas que actualizar la página o esperar unos minutos para ver el cambio.

13. En Azure Portal, ve a la página **Grupos - todos los grupos** del inquilino de Contoso Azure AD y ten en cuenta que todos los grupos de dominio de corp.contoso.com también se han sincronizado. 

14. En Azure Portal, ve a la página **Contoso - Azure AD Connect** y selecciona **Azure AD Connect** a la izquierda. Comprueba que la configuración siguiente está activa: 

    - Estado de sincronización de Azure AD Connect: **habilitado** 
  
    - Última sincronización: **debe ser una marca de tiempo de algún tipo**. 
  
    - Sincronización de hash de contraseñas: **deshabilitada** 
  
    - Federación: **Deshabilitado**
   
    - Inicio de sesión único de conexión directa: **habilitado para un dominio** 
  
    - Autenticación transferida: **Habilitada con 1 agente**

   > **Nota**: En un entorno de producción, instalarías agentes adicionales para la redundancia. Para obtener más información, vea <https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-pta-quick-start>.

### Tarea 10: configurar la unión a Azure AD híbrido

En esta tarea, configurarás opciones de sincronización de dispositivos de Azure AD Connect.

1. En la sesión de Escritorio remoto a **DC1**, haz doble clic en el acceso directo de escritorio de **Azure AD Connect**.

2. En la página **Bienvenido a Azure AD Connect**, haz clic en **Configurar**. 

3. En la página **Tareas adicionales**, selecciona **Configurar opciones de dispositivo** y luego selecciona **Siguiente**.

4. En la página **Información general** revisa la información relativa a **Unión a Azure AD híbrido** y a **Escritura diferida de dispositivo** y selecciona **Siguiente**.

5. En la página **Conectar a Azure AD**, inicia sesión con las credenciales de la cuenta **john.doe** y selecciona **Siguiente**.

6. En la página **Opciones de dispositivos**, asegúrate de que la opción **Configurar unión a Azure AD híbrido** está seleccionada y después selecciona **Siguiente**. 

7. En la página **Sistemas operativos del dispositivo**, selecciona las casillas **Dispositivos unidos a un dominio de Windows 10 o posterior** y **Dispositivos unidos a un dominio de Nivel inferior compatibles con Windows ** y selecciona **Siguiente**. 

   > **Nota**: Los dispositivos de nivel inferior de Windows solo se admiten si usas SSO de conexión directa para dominios administrados o un servicio de federación como AD FS para dominios federados.

8. En la página **Configuración de SCP**, activa el cuadro de diálogo **corp.contoso.com** Bosque de Active Directory, selecciona la entrada **.Azure Active Directory** en la lista desplegable **Servicio de autenticación** y selecciona **Agregar**.

9. Cuando se te soliciten credenciales de Administrador de organización para corp.contoso.com, en el cuadro de diálogo **Seguridad de Windows**, inicia sesión con el nombre de usuario **demouser** CORP\\ y la contraseña **demo\@pass123**.

10. En la página **Configuración de SCP**, haz clic en **Siguiente**.

11. En la página **Listo para configurar**, seleccione **Configurar**.

12. En la página **Configuración completa**, comprueba que la tarea se ha completado correctamente y selecciona **Salir**.


### Tarea 11: realizar una unión a Azure AD híbrido

1. En el equipo del laboratorio, en Azure Portal, comprueba que has iniciado sesión en el inquilino de Azure AD asociado a la suscripción de Azure en la que has implementado recursos en los ejercicios del laboratorio práctico previos (el **Directorio predeterminado**). Si no es así, selecciona el icono **Directorio y suscripción** de la barra de herramientas de Azure Portal (a la derecha del icono de **Cloud Shell**) para cambiar a ese inquilino de Azure AD. 

2. En Azure Portal, ve a la página de la máquina virtual **APP1**.

3. En la página de la máquina virtual ** APP1**, conéctate a **APP1** a través de Escritorio remoto. Cuando se te pida que inicies sesión, usa el nombre de usuario **AGAyers\@<custom_domain_name>**  con la contraseña **demo@pass123** (donde el marcador de posición  **<custom_domain_name>**  representa el nombre de dominio DNS personalizado que has asignado al inquilino de Azure AD de Contoso anteriormente en este ejercicio.

4. En la sesión de Escritorio remoto a **APP1**, en la ventana **Administrador del servidor**, inicia el **Programador de tareas** en **Herramientas**. 


5. En la consola del **Programador de tareas**, ve a **Biblioteca de Programador de tareas** > **Microsoft** > **Windows** > **Workplace Join**. Desde allí, habilita y ejecuta la tarea **Automatic-Device-Join**. 


6. Cambia a la sesión de Escritorio remoto a **DC1** y, desde el panel de consola de la ventana de Windows PowerShell ISE, inicia la sincronización diferencial de Azure AD Connect ejecutando lo siguiente:

   ```pwsh
   Import-Module -Name 'C:\Program Files\Microsoft Azure AD Sync\Bin\ADSync\ADSync.psd1'
   
   Start-ADSyncSyncCycle -PolicyType Delta
   ```

7. Vuelve a la sesión de Escritorio remoto a **APP1** e inicia un **símbolo del sistema**.

8. En la ventana del símbolo del sistema, comprueba el estado de registro de Azure AD de APP1 ejecutando lo siguiente: 

   ```
   dsregcmd /status
   ```

9. Verifica que la salida del comando sea similar a lo siguiente:

   ```
   +----------------------------------------------------------------------+
   | Device State                                                         |
   +----------------------------------------------------------------------+

        AzureAdJoined : YES
     EnterpriseJoined : NO
             DeviceId : 61eea2b8-efbe-43d9-b267-126433c8ee34
           Thumbprint : BBAAA0FB4A55E880388851BED955A2669A961A96
       KeyContainerId : 2eb75eb8-0a1d-437b-99d9-9dd161ca0d90
          KeyProvider : Microsoft Software Key Storage Provider
         TpmProtected : NO
         KeySignTest: : PASSED
                  Idp : login.windows.net
             TenantId : xxxxxxx-xxxx-xxx-xxxx-xxxxxxxxxx
           TenantName : xxxxxxx-xxxx-xxx-xxxx-xxxxxxxxxx
          AuthCodeUrl : https://login.microsoftonline.com/xxxxxxx-xxxx-xxx-xxxx-xxxxxxxxxx/oauth2/authorize
       AccessTokenUrl : https://login.microsoftonline.com/xxxxxxx-xxxx-xxx-xxxx-xxxxxxxxxx/oauth2/token
               MdmUrl :
            MdmTouUrl :
     MdmComplianceUrl :
          SettingsUrl :
       JoinSrvVersion : 1.0
           JoinSrvUrl : https://enterpriseregistration.windows.net/EnrollmentServer/device/
            JoinSrvId : urn:ms-drs:enterpriseregistration.windows.net
        KeySrvVersion : 1.0
            KeySrvUrl : https://enterpriseregistration.windows.net/EnrollmentServer/key/
             KeySrvId : urn:ms-drs:enterpriseregistration.windows.net
         DomainJoined : YES
           DomainName : CORP

   +----------------------------------------------------------------------+
   | User State                                                           |
   +----------------------------------------------------------------------+

               NgcSet : NO
      WorkplaceJoined : NO
        WamDefaultSet : NO
           AzureAdPrt : NO

   +----------------------------------------------------------------------+
   | Ngc Prerequisite Check                                               |
   +----------------------------------------------------------------------+

        IsUserAzureAD : NO
        PolicyEnabled : NO
       DeviceEligible : YES
   SessionIsNotRemote : NO
     X509CertRequired : NO
         PreReqResult : WillNotProvision

   ```
11. Vuelva a la sesión de Escritorio remoto a **DC1**, en la ventana del explorador Edge que muestra Azure Portal, ve a la página **Dispositivos - Todos los dispositivos** del inquilino de Azure AD de Contoso y comprueba que hay una entrada que representa el servidor APP1, con el **Tipo de unión** establecido **en Unido a Azure AD híbrido**.

   > **Nota**: Es posible que tengas que esperar hasta que se notifique correctamente el estado de registro de Azure AD y su objeto de Azure AD aparezca en Azure Portal.


