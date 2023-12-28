---
lab:
  title: 07 - Agregar una identidad híbrida con Azure AD Connect
  learning path: '01'
  module: Module 01 - Implement an identity management solution
---

# Laboratorio 07: OPCIONAL --- Agregar una identidad híbrida con Azure AD Connect

**Nota:** este laboratorio requiere un pase para Azure. Consulta el laboratorio 00 para obtener indicaciones.

**Nota 2**: este laboratorio se llama Opcional.  Tarda al menos 1 hora en completarse y requiere que prestes atención en los pasos del laboratorio.  No dudes en computarlo si dispones de tiempo.  Si tu empresa ya ha establecido su configuración híbrida o no tiene previsto usar Azure AD Connect, ve a este laboratorio.

## Escenario del laboratorio

Los trabajadores de tu empresa tienen Active Directory Domain Services local.  Les gustaría seguir usando Active Directory local como su solución de administración de identidades y acceso, pero también necesitan que los usuarios accedan a las aplicaciones en la nube con el mismo nombre de usuario y contraseña.

#### Tiempo estimado: 60 minutos

### Ejercicio 1: configuración de la infraestructura local

#### Tarea 1: crear la infraestructura de Active Directory local

1. Se puede acceder a la plantilla de implementación en este vínculo: [Guía de laboratorio de prueba local](https://github.com/maxskunkworks/TLG/tree/master/tlg-base-config_3-vm).

    **Nota para los alumnos y MCT**: la implementación de esta plantilla puede durar entre 30 y 60 minutos, así que prepárate para hacer una pausa en este paso o ejecuta la implementación antes de una sección de lecciones del curso.

    **Nota para los proveedores de los laboratorios**: si es posible, sería útil que lo alumnos lo completaran e implementaran como parte de la configuración del entorno del laboratorio.

2. En la página **TLG (Guía de laboratorio de prueba local): configuración base de 3 máquinas virtuales (v1.0)**, selecciona **Implementar en Azure**.

   **Nota**: la configuración base de 3 máquinas virtuales aprovisiona un controlador de dominio de Active Directory de Windows Server 2016 denominado DC1 con el nombre de dominio que especifiques y un servidor miembro de dominio denominado APP1 que ejecuta Windows Server 2016. También ofrece una opción para aprovisionar una máquina virtual cliente que ejecuta Windows 10, pero no la usaremos en nuestro laboratorio (básicamente debido a los requisitos de licencia aplicables al ejecutar máquinas virtuales de Windows 10 en Azure). El servidor miembro del dominio (APP1) ha instalado automáticamente .NET 4.5 e IIS.  
   
   **Nota**: la máquina virtual requerida para este laboratorio es **DC1**.  Si usas un pase para Azure, hay una limitación de 2 máquinas virtuales, por lo que es posible que se produzca un error en la máquina virtual cliente.  No es necesario para este laboratorio.

3. En la página **Implementación personalizada**, especifica la siguiente configuración y después selecciona **Revisar y crear**, y luego **Crear**.

   -   Suscripción: el nombre de la suscripción de Azure de destino donde deseas aprovisionar las máquinas virtuales de Azure del entorno de laboratorio.
   -   Grupo de recursos: (crear nuevo) **hybrididentity-RG**
   -   Ubicación: el nombre de la región de Azure que hospedará las máquinas virtuales de Azure del entorno de laboratorio.
   -   Nombre de configuración: **TlgBaseConfig-01**
   -   Nombre del dominio: **corp.contoso.com**
   -   Sistema operativo del servidor: **2016-Datacenter**
   -   Nombre de usuario de administrador: **demouser**
   -   Contraseña de administrador: **introduce una contraseña segura que te sea fácil de recordar**
   -   Implementar máquina virtual cliente: **no**
   -   URI de VHD del cliente: **dejar en blanco**
   -   Tamaño de la máquina virtual: **Standard_D2s_v3**
   
   **Nota**: usa un tamaño de máquina virtual similar si tu suscripción no es compatible con el tamaño indicado. La documentación está vinculada aquí: <https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes>.

   -   Prefijo de etiqueta DNS: **cualquier nombre DNS válido y único global (una cadena única formada por letras, dígitos y guiones, que empieza por una letra y tiene hasta 47 caracteres de longitud).**

   -   Ubicación de _artifacts: **aceptar la predeterminada**
   -   Token de SAS de ubicación de _artifacts: **dejar en blanco**

4. Seleccione **Revisar + crear**.

5. Seleccione **Crear** una vez que se pase la validación.
    
6. Espere a que la implementación se complete. Esto puede tardar unos 60 minutos.


### Tarea 2: configurar el entorno de laboratorio de las máquinas virtuales de Azure

1. En la ventana del explorador que muestra Azure Portal, ve a la máquina virtual de Azure **DC1** y conéctate a ella con Escritorio remoto. Cuando el sistema lo solicite, inicia sesión con las siguientes credenciales:

   -   Nombre de usuario: **demouser**
   -   Contraseña: **usa la contraseña segura que has creado en la tarea 1**

2.  En la sesión de Escritorio remoto de **DC1**, inicia **Windows PowerShell ISE**, agrega el siguiente script al panel de scripts y ejecútalo para deshabilitar la configuración de seguridad mejorada de Internet Explorer y el control de acceso de usuario en máquinas virtuales de Azure **DC1** y **APP1**:

    ```pwsh

    $vmNames = @('dc1','app1')
    Invoke-Command -ComputerName $vmNames {Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A7-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0}
    Invoke-Command -ComputerName $vmNames {Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A8-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0}
    Invoke-Command -ComputerName $vmNames {Set-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" -Name "ConsentPromptBehaviorAdmin" -Value 00000000}
    ```

    **Nota:** para ejecutar varios scripts de PowerShell en el mismo archivo, puedes resaltar un script específico y seleccionar **Ejecutar selección** junto al botón de reproducción verde. 

3.  En la ventana de **Windows PowerShell ISE**, agrega el siguiente script al panel de scripts y ejecútalo para instalar Herramientas de administración remota del servidor en máquinas virtuales de Azure **DC1* y **APP1**:

    ```pwsh

    $vmNames = @('dc1','app1')
    Invoke-Command -ComputerName $vmNames {Install-WindowsFeature RSAT -IncludeAllSubFeature} 
    ```

4.  En la ventana de **Windows PowerShell ISE**, agrega el siguiente script al panel de scripts y ejecútalo para habilitar TLS 1.2 en las máquinas virtuales de Azure **DC1* y **APP1**:

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

5.  En la ventana de **Windows PowerShell ISE**, agrega el siguiente script al panel de scripts y ejecútalo para configurar la autenticación integrada de Windows en el sitio web predeterminado hospedado en la máquina virtual de Azure **APP1**:

    ```pwsh

    $vmNames = @('app1')
    Invoke-Command -ComputerName $vmNames {Enable-WindowsOptionalFeature -Online -FeatureName IIS-WindowsAuthentication}
    Invoke-Command -ComputerName $vmNames {Set-WebConfigurationProperty -Filter "/system.webServer/security/authentication/anonymousAuthentication" -Name Enabled -Value False -PSPath IIS:\ -Location "Default Web Site"}
    Invoke-Command -ComputerName $vmNames {Set-WebConfigurationProperty -Filter "/system.webServer/security/authentication/windowsAuthentication" -Name Enabled -Value True -PSPath IIS:\ -Location "Default Web Site"}
    ```

### Tarea 3: reiniciar las máquinas virtuales de Azure

1. En la ventana de **Windows PowerShell ISE**, en el panel de consola, ejecuta lo siguiente para reiniciar **APP1**:

    ```pwsh

    Restart-Computer -ComputerName 'APP1'
    ```

2. En la ventana de **Windows PowerShell ISE**, en el panel de consola, ejecuta lo siguiente para reiniciar **DC1**:

    ```pwsh
    Restart-Computer -ComputerName 'DC1'
    ```

### Tarea 5: configurar contoso.local de Active Directory

1. Conéctate de nuevo a la máquina virtual **DC1** de Azure a través de Escritorio remoto. Cuando el sistema lo solicite, inicia sesión con las siguientes credenciales:

    -   Nombre de usuario: **demouser**

    -   Contraseña: **demo\@pass123**
       - **Se recomienda encarecidamente introducir una contraseña segura que puedas recordar.**

2.  Dentro de la sesión de Escritorio remoto de **DC1**, inicia Internet Explorer y ve al siguiente vínculo.

    ```
    https://github.com/microsoft/MCW-Hybrid-identity/tree/main/Hands-on%20lab/studentfiles
    ```

3. En la página **Crear usuarios/Grupo para la demostración de Active Directory/Entorno de pruebas**, selecciona el vínculo **CreateDemoUsers.ps1**, acepta los términos de licencia y guarda el script correspondiente en el sistema de archivos local.

4. En la página **Crear usuarios/Grupo para la demostración de Active Directory/Entorno de pruebas**, selecciona el vínculo **CreateDemoUsers.csv** (directamente encima de la sección de código de PowerShell) y guarda el archivo CSV correspondiente en la misma ubicación que el archivo **CreateDemoUsers.ps1**.

5. Dentro de la sesión de Escritorio Remoto de **DC1**, inicia el Explorador de Archivos, ve a la carpeta donde has descargado ambos archivos, selecciona con el botón derecho del ratón el archivo **CreateDemoUsers.ps1**, selecciona **Propiedades**, en el cuadro de diálogo **Propiedades de CreateDemoUsers.ps1**, marca la casilla **Desbloquear** y selecciona **Aceptar**.

6. Dentro de la ventana del Explorador de archivos, vuelve a seleccionar con el botón derecho del ratón el archivo **CreateDemoUsers.ps1** y selecciona **Editar**. 

7. En la ventana **Administrador: Windows PowerShell ISE**, cambia la línea **148** de:

    ```pwsh
    $UserCount = 1000 #Up to 2500 can be created
    ```

   to
    ```pwsh
    $UserCount = 2500 #Up to 2500 can be created
    ```

8. En la ventana **Windows PowerShell ISE**, guarda el cambio y ejecuta el script **CreateDemoUsers.ps1** para crear una jerarquía de unidades organizativas de entorno de laboratorio y rellenarla con cuentas de usuario de prueba. 

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

10. En la ventana de **Windows PowerShell ISE**, agrega el siguiente script al panel de scripts y ejecútalo para crear unidades organizativas adicionales denominadas **Servidores** y **Clientes**, y mover la cuenta de equipo **APP1** al primero de ellos:

    ```pwsh

    New-ADOrganizationalUnit -Name 'Servers' -Path 'OU=Demo Accounts,DC=corp,DC=contoso,DC=com'
    New-ADOrganizationalUnit -Name 'Clients' -Path 'OU=Demo Accounts,DC=corp,DC=contoso,DC=com'

    Move-ADObject -Identity 'CN=APP1,CN=Computers,DC=corp,DC=contoso,DC=com' -TargetPath 'OU=Servers,OU=Demo Accounts,DC=corp,DC=contoso,DC=com'
    ```

11. Cierra sesión en **DC1**.

## Ejercicio 2: integrar un bosque de Active Directory con un inquilino de Azure Active Directory

### Tarea 1: crear un inquilino de Azure Active Directory y habilitar una prueba de EMS E5

En esta tarea, crearás un inquilino de Azure Active Directory con la siguiente configuración: 

-   Nombre de la organización: **Contoso**

-   Nombre del dominio inicial: cualquier nombre de dominio válido y único.

-   País o región: **Estados Unidos**

1. En el equipo del laboratorio, inicia una nueva ventana del explorador web y ve a Azure Portal en <https://portal.azure.com> si aún no lo has hecho.

2. Cuando se te solicite, inicia sesión en la suscripción de Azure en la que has implementado recursos en los ejercicios del laboratorio anterior a la práctica.

3. En el ordenador del laboratorio, en Azure portal, selecciona **+ Crear un recurso**.

4. En la página **Nuevo**, en el cuadro de texto **Buscar en Marketplace**, escribe **Azure Active Directory** y, en la lista de resultados, selecciona **Azure Active Directory**.

5. En la página **Azure Active Directory**, selecciona **Crear**.

6. En la página **Crear directorio**, especifica la siguiente configuración y selecciona **Crear**:

Pestaña Básico:
    -   Seleccionar un tipo de inquilino: selecciona **Azure Active Directory**

Pestaña Configuración:
    -   Nombre de la organización: **Contoso**

    -   Nombre del dominio inicial: cualquier nombre de dominio válido y único.

    -   País o región: **Estados Unidos**

7. Una vez creado, abre **Azure Active Directory**.

8. En la página Información general, selecciona **Administrar inquilino**.

9. Coloca una marca de verificación en el directorio recién creado.

10. En la parte superior de la página, elige **Cambiar**.

    >**Nota**: pueden pasar unos minutos hasta que todo se muestre correctamente.

11. En la página **Contoso: información general**, selecciona **Usuarios**.

12. Solo tienes un único usuario ExternalAzureAD en este nuevo inquilino.

### Tarea 2: crear y configurar un usuario de Azure AD para administrar este directorio

1. En el equipo del laboratorio, en Azure Portal, vuelve a la página **Contoso: información general**.

2. En la página **Contoso: información general**, selecciona **Usuarios** en **Administrar** en la navegación izquierda.

3. En la página **Usuarios: todos los usuarios**, selecciona la entrada que representa tu cuenta de usuario.

4. En la página **Perfil** de tu cuenta de usuario, selecciona **Editar**.

5. En la sección **Configuración**, en la lista desplegable **Ubicación de uso**, selecciona la entrada **Estados Unidos** y selecciona **Guardar**.

#### Crear el nuevo administrador

6. En la página **Nuevo usuario**, asegúrate de que la opción **Crear usuario** esté seleccionada, especifica la siguiente configuración y selecciona **Crear**:

    - Nombre de usuario: **john.doe *@yournombre de dominio del inquilino de Azure AD*** donde el ***nombre de dominio de su inquilino de Azure AD*** es el nombre de dominio que has especificado al crear el inquilino de Azure AD de Contoso.

    - Nombre: **john.doe**

    - Nombre: **John**

    - Apellido: **Doe**
    
    - Contraseña: **Generar automáticamente la contraseña**
    
    - Mostrar contraseña: **habilitada** y, después, asegúrate de copiar la contraseña.

    - Grupos: **0 grupos seleccionados**
    
    - Roles: **administrador global**
    
    - Bloquear inicio de sesión: **No**
    
    - Ubicación de uso: **Estados Unidos**
    
    - Puesto: **dejar en blanco**
    
    - Departamento: **dejar en blanco**

    > **Nota**: copia los valores **Nombre de usuario** y **Contraseña** en el Bloc de notas. Los necesitará más adelante en este laboratorio.


### Tarea 5: configurar el sufijo DNS en el bosque de Contoso Active Directory

En esta tarea, configurarás el sufijo DNS del bosque de Contoso Active Directory para que coincida con el nombre de dominio personalizado de Azure AD recién comprobado.

1. En el equipo de laboratorio, en Azure Portal, comprueba que has iniciado sesión en el inquilino de Azure AD asociado a la suscripción de Azure en la que has implementado recursos en los ejercicios del laboratorio anterior a la práctica (el **directorio predeterminado**). Si no es así, selecciona el icono **Directorio + suscripción** de la barra de herramientas de Azure Portal (a la derecha del icono de **Cloud Shell**) para cambiar a ese inquilino de Azure AD. 

2. En Azure Portal, ve a la página de la máquina virtual **DC1**.

3. En la página de la máquina virtual **DC1**, conéctate a **DC1** con Escritorio remoto. Cuando debas iniciar sesión, usa el nombre **demouser** y la contraseña **demo\@pass123**. 

4. Dentro de la sesión de Escritorio remoto de **DC1**, en la ventana **Administrador del servidor**, inicia la consola **Dominios y confianzas de Active Directory** en **Herramientas**. 

5. En la consola **Dominios y confianzas de Active Directory**, haz clic con el botón derecho en **Dominios y confianzas de Active Directory [DC1.corp.contoso.com]** a la izquierda y selecciona **Propiedades**.

6. En la pestaña **Sufijos UPN** de la ventana **Dominios y confianzas de Active Directory [DC1.corp.contoso.com]**, en el cuadro de texto **Sufijos UPN alternativos**, escribe el nombre del dominio personalizado que has comprobado en la tarea anterior. Selecciona **Agregar** y después selecciona **Aceptar**.

7. En la sesión de Escritorio remoto de **DC1**, en la ventana **Administrador del servidor**, inicia la consola **Usuarios y equipos de Active Directory** en **Herramientas**. 

8. En la consola **Usuarios y equipos de Active Directory**, expande **corp.contoso.com** a la izquierda y examina la jerarquía de unidades organizativas del dominio y la pertenencia al grupo de los grupos de dominios. 

9.  En la sesión de Escritorio remoto de **DC1**, inicia Windows PowerShell ISE y, en el panel Script, ejecuta lo siguiente para reemplazar el sufijo UPN de todos los usuarios que son miembros del grupo de **ingeniería** por el que coincida con el nombre de dominio comprobado personalizado del inquilino de Azure AD de Contoso (reemplaza el marcador de posición `<custom_domain_name>` por el nombre de dominio comprobado personalizado que has asignado al inquilino de Azure AD de Contoso). 

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

### Tarea 6: instalar Azure AD Connect

En esta tarea, instalarás Azure AD Connect.

1. En la sesión de Escritorio remoto de **DC1**, en Administrador del servidor, selecciona **Servidor local** y asegúrate de que la **configuración de seguridad mejorada de Internet Explorer** esté deshabilitada. Si no es así, selecciona el vínculo **Activado** junto a la **configuración de seguridad mejorada de Internet Explorer**, establece la configuración **Administradores** en **Desactivado** y selecciona **Aceptar**.

2. En la sesión de Escritorio remoto de **DC1**, abre la ventana **Windows PowerShell ISE** y ejecuta este comando para instalar el explorador Chrome.

    ```pwsh
    $LocalTempDir = $env:TEMP; $ChromeInstaller = "ChromeInstaller.exe"; (new-object System.Net.WebClient).DownloadFile('http://dl.google.com/chrome/install/375.126/chrome_installer.exe', "$LocalTempDir\$ChromeInstaller"); & "$LocalTempDir\$ChromeInstaller" /silent /install; $Process2Monitor = "ChromeInstaller"; Do { $ProcessesFound = Get-Process | ?{$Process2Monitor -contains $_.Name} | Select-Object -ExpandProperty Name; If ($ProcessesFound) { "Still running: $($ProcessesFound -join ', ')" | Write-Host; Start-Sleep -Seconds 2 } else { rm "$LocalTempDir\$ChromeInstaller" -ErrorAction SilentlyContinue -Verbose } } Until (!$ProcessesFound)
    ```

2. En la sesión de Escritorio remoto de **DC1**, inicia el explorador Chrome y ve a Azure Portal en <https://portal.azure.com>.

3. Cuando el sistema solicite iniciar sesión, escribe las credenciales de la cuenta del usuario **john.doe** de Azure AD, que has copiado en el Bloc de notas anteriormente en este ejercicio.

4. Cuando el sistema lo solicite, cambia la contraseña de la cuenta del usuario **john.doe**. 
  
    > **Nota**: si recibes el mensaje **Ya hemos visto esa contraseña demasiadas veces. Elige algo más difícil de adivinar**, tendrás que modificar la contraseña hasta que sea lo suficientemente única como para ser aceptada.

5. Si el sistema te pregunta si deseas **mantener la sesión iniciada** seleccione **No**. Se te redirigirá a la interfaz de Azure Portal. 

6. Si se muestra el cuadro de diálogo **Le damos la bienvenida a Microsoft Azure**, selecciona **Quizás más tarde**. 

7. En Azure Portal, selecciona **Azure Active Directory** en la navegación izquierda del portal para ir a la página **Contoso: información general**.

8. En la página **Contoso: información general**, selecciona **Azure AD Connect** en **Administrar** a la izquierda.

9.  En la hoja **Azure AD Connect**, selecciona el vínculo **Descargar Azure AD Connect**.  Después elige **Conectar sincronización** del menú.

10. En la página web **Microsoft Azure Active Directory Connect** del sitio de descargas de Microsoft, selecciona **Descargar**.

11. Cuando el sistema te pregunte si deseas ejecutar o guardar **AzureADConnect.msi**, selecciona **Ejecutar**. De este modo se descargará el archivo e iniciará automáticamente el asistente **Microsoft Azure Active Directory Connect**. 

12. En la página **Le damos la bienvenida a Azure AD Connect**, marca la casilla **Acepto los términos de licencia y el aviso de privacidad**, y selecciona **Continuar**.

13. En la página **Configuración de Express**, selecciona el botón **Personalizar**.

14. En la página **Instalar componentes necesarios**, deje desactivadas todas las opciones de configuración opcionales y seleccione **Instalar**.

15. En la página **Inicio de sesión de usuario**, selecciona la opción **Autenticación transferida** y las casillas **Habilitar el inicio de sesión único**, y selecciona **Siguiente**.

16. En la página **Conectarse a Azure AD**, inicia sesión con las credenciales de la cuenta de **john.doe** y selecciona **Siguiente**.

17. En la página **Conectar los directorios**, asegúrate de que la entrada **corp.contoso.com** aparece en la lista desplegable **BOSQUE** y selecciona **Agregar directorio**. En la **cuenta del bosque AD**, asegúrate de que está seleccionada la opción **Crear nueva cuenta de AD**, en el cuadro de texto **ENTERPRISE ADMIN USERNAME**, escribe **CORP. CONTOSO.COM\\demouser**, en el cuadro de texto **PASSWORD**, escribe **demo\@pass123**, y selecciona **Aceptar**.


18. De vuelta en la página **Conecta tus directorios**, selecciona **Siguiente**.

19. En la página **Configuración de inicio de sesión de Azure AD**, asegúrate de que el nombre de dominio personalizado aparezca como **Sufijo UPN de Active Directory** comprobado y de que la entrada **userPrincipalName** aparezca en la lista desplegable **USER PRINCIPAL NAME**. Observa la advertencia que indica **Los usuarios no podrán iniciar sesión en Azure AD con credenciales locales si el sufijo UPN no coincide con un nombre de dominio comprobado**. Marca la casilla **Continuar sin hacer coincidir todos los sufijos UPN con el dominio comprobado** y selecciona **Siguiente**. 

    >**Nota**: esto es normal, ya que algunos usuarios todavía están configurados con el sufijo UPN **contoso.local**, que no es enrutable y no se puede configurar como nombre de dominio personalizado comprobado de un inquilino de Azure AD.

20. En la página **Filtrado de dominios y unidades organizativas**, elige **Sincronizar dominios y unidades organizativas seleccionadas**, asegúrate de que solo estén seleccionadas la unidad organizativa **Cuentas de demostración** y todas sus unidades organizativas secundarias, y selecciona **Siguiente**. 


21. En la página **Identificar de forma exclusiva a sus usuarios**, acepta la configuración predeterminada y selecciona **Siguiente**. 


22. En la página **Filtrar usuarios y dispositivos**, acepta la configuración predeterminada y selecciona **Siguiente**. 

23. En la página **Características opcionales**, acepta la configuración predeterminada y selecciona **Siguiente**.

24. En la página **Habilitar inicio de sesión único**, selecciona **Introducir credenciales**, en el cuadro de diálogo **Credenciales del bosque**, inicia sesión con el nombre de usuario **CORP\\demouser** y la contraseña **demo\@pass123**, y selecciona **Siguiente**.


25. En la página **Listo para configurar**, asegúrate de que la casilla **Iniciar el proceso de sincronización cuando finalice la configuración** esté en **NO** y selecciona **Instalar**.


   > **Nota**: configurarás el filtrado de nivel de atributo antes de habilitar el proceso de sincronización.

   > **Nota**: La instalación debería tardar aproximadamente 2 minutos.

26. En la página **Configuración completada**, seleccione **Salir**.


### Tarea 7: habilitar la papelera de reciclaje de Active Directory

En esta tarea, habilitarás la Papelera de reciclaje en el dominio Contoso de Active Directory. 

1. Dentro de la sesión de Escritorio remoto de **DC1**, en el menú Herramientas de la consola Administrador del servidor, inicia **Centro de administración de Active Directory**.


2. En la consola **Centro de administración de Active Directory**, haz clic con el botón derecho en **corp (local)** a la izquierda y selecciona **Habilitar papelera de reciclaje**. Cuando se le pida confirmación, seleccione **Aceptar**.


3. Cuando el sistema te solicite actualizar el Centro de administración de AD, selecciona **Aceptar**.

   > **Nota**: para obtener información sobre las ventajas de la papelera de reciclaje en escenarios híbridos, consulta <https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-sync-recycle-bin>

### Tarea 8: configurar el filtrado de nivel de atributo de Azure AD

En esta tarea, configurarás el filtrado de nivel de atributo de Azure AD Connect que limitará la sincronización de cuentas de usuario con el sufijo UPN que coincida con el nombre de dominio personalizado del inquilino de Azure AD de destino.

   > **Nota**: la opción de filtrado positivo requiere al menos dos reglas de sincronización. Uno de ellos determina el ámbito correcto de los objetos que se van a sincronizar. La segunda regla de sincronización de comodín filtra todos los objetos que aún no han sido identificados como objetos que deben sincronizarse.

1. En la sesión de Escritorio remoto de **DC1**, inicia el **Editor de reglas de sincronización** de **Azure AD Connect** en el menú Inicio.


2. En la ventana Editor de reglas de sincronización, en la página **Ver y administrar tus reglas de sincronización**, asegúrate de que **Entrada** aparece en la lista desplegable **Dirección** y selecciona **Agregar nueva regla**. Esto iniciará el asistente **Crear regla de sincronización de entrada**.


3. En la página **Crear regla de sincronización entrante: descripción**, especifica la siguiente configuración y selecciona **Siguiente**:

    - Nombre: **Custom In from AD - UPN Filter**

    - Descripción: la **regla de entrada personalizada: incluye usuarios con UPN configurado para que coincida con el dominio personalizado de Azure AD**.

    - Sistema conectado: **corp.contoso.com**

    - Tipo de objeto de sistema conectado: **usuario**

    - Tipo de objeto de metaverso: **persona**

    - Tipo de vínculo: **unirse**

    - Precedencia: **50**

    - Etiqueta: **dejar vacío**

    - Habilitar sincronización de contraseñas: **dejar vacío**

    - Deshabilitado: **dejar vacío**


4. En la página **Crear filtro de ámbito de entrada**, selecciona **Agregar grupo**, selecciona **Agregar cláusula** especifica lo siguiente y selecciona **Siguiente**:

    - Atributo: **userPrincipalName**

    - Operador: **ENDSWITH**

    - Valor: **\@\<your custom domain name>**

5. En la página **Unir reglas**, selecciona **Siguiente**.

6. En la página **Transformaciones**, selecciona **Agregar transformación** especifica lo siguiente y selecciona **Agregar**:

    - FlowType: **constante**

    - Atributo de destino: **cloudFiltered**

    - Origen: **falso**

7. Cuando aparezca el cuadro de diálogo de **Advertencia** que muestra un mensaje que indica que se ejecutará **una importación y sincronización completas en "corp.contoso.com" durante el siguiente ciclo de sincronización**, selecciona **Aceptar**.

   > **Nota**: esto debería devolverte a la vista y administrar tu interfaz de reglas de sincronización, con la nueva regla que se muestra en la parte superior de la lista de reglas. 

8. De nuevo en la ventana **Editor de reglas de sincronización**, en la página **Ver y administrar sus reglas de sincronización**, asegúrate de que **Entrada** aparezca en la lista desplegable **Dirección** y de seleccionar de nuevo **Agregar nueva regla**. Esto iniciará el asistente **Crear regla de sincronización de entrada**.

9. En la página **Descripción**, especifica la siguiente configuración y selecciona **Siguiente**:

    - Nombre: **Custom In from AD - Catch-all Filter**

    - Descripción: **regla de entrada personalizada: se excluyen todos los usuarios cuyo UPN no coincida con el dominio personalizado de Azure AD**.

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

    - FlowType: **constante**

    - Atributo de destino: **cloudFiltered**

    - Origen: **verdadero**

13. Cuando aparezca el cuadro de diálogo de **Advertencia** que muestra un mensaje que indica que se ejecutará **una importación y sincronización completa en "corp.contoso.com" durante el siguiente ciclo de sincronización**, selecciona **Aceptar**.

    >**Nota**: esto debería devolverte a la **vista y administrar tu interfaz de reglas de sincronización**, con la nueva regla que se muestra en la parte superior de la lista de reglas. 

### Tarea 9: iniciar y comprobar la sincronización de directorios

1. En la sesión de Escritorio remoto de **DC1**, haz doble clic en el acceso directo de escritorio de **Azure AD Connect**.

2. En la página **Le damos la bienvenida a Azure AD Connect**, selecciona **Configurar**. 

3. En la página **Tareas adicionales**, selecciona **Personalizar opciones de sincronización** y luego, **Siguiente**.

4. En la página **Conectarse a Azure AD**, inicia sesión con las credenciales de la cuenta de **john.doe** y selecciona **Siguiente**.

5. En la página **Conectar sus directorios**, haga clic en **Siguiente**.

6. En la pantalla **Filtrado de dominios y unidades organizativas**, seleccione **Siguiente**. 

7. En la página **Características opcionales**, acepta la configuración predeterminada y selecciona **Siguiente**.

8. En la página **Habilitar inicio de sesión único**, selecciona **Siguiente**.

9. En la página **Listo para configurar**, selecciona la casilla **Iniciar el proceso de sincronización cuando finalice la configuración** y selecciona **Configurar**.

10. En la página **Configuración completada**, seleccione **Salir**.

11. En la sesión de Escritorio remoto de **DC1**, en la ventana del explorador Edge que muestra Azure Portal, ve a la página **Usuarios: todos los usuarios** del inquilino de Azure AD de Contoso.

12. En la página **Usuarios: todos los usuarios**, observa que la lista de objetos de usuario incluye todas las cuentas de usuario con el sufijo UPN que coinciden con el nombre de dominio personalizado del inquilino de Azure AD. Es posible que tengas que actualizar la página o esperar unos minutos para ver el cambio.

13. En Azure Portal, ve a la página **Grupos: todos los grupos** del inquilino de Azure AD de Contoso y observa que todos los grupos de dominio de corp.contoso.com también se han sincronizado. 

14. En Azure Portal, ve a la página **Contoso: Azure AD Connect** y selecciona **Azure AD Connect** a la izquierda. Comprueba que se haya establecido la configuración siguiente: 

    - Estado de sincronización de Azure AD Connect: **habilitado** 
  
    - Última sincronización: **debe ser una marca de tiempo de algún tipo**. 
  
    - Sincronización de hash de contraseñas: **deshabilitada** 
  
    - Federación: **deshabilitada**
   
    - Inicio de sesión único de conexión directa: **habilitado para un dominio** 
  
    - Autenticación transferida: **habilitado con 1 agente**

   > **Nota**: en un entorno de producción, instalarías agentes adicionales para la redundancia. Para obtener más información, vea <https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-pta-quick-start>.

### Tarea 10: configurar la unión híbrida de Azure AD

En esta tarea, configurarás opciones de sincronización de dispositivos de Azure AD Connect.

1. En la sesión de Escritorio remoto de **DC1**, haz doble clic en el acceso directo de escritorio de **Azure AD Connect**.

2. En la página **Le damos la bienvenida a Azure AD Connect**, selecciona **Configurar**. 

3. En la página **Tareas adicionales**, selecciona **Configurar opciones del dispositivo** y selecciona **Siguiente**.

4. En la página **Información general**, revisa la información sobre la **unión híbrida de Azure AD** y la **escritura diferida**, y selecciona **Siguiente**.

5. En la página **Conectarse a Azure AD**, inicia sesión con las credenciales de la cuenta de **john.doe** y selecciona **Siguiente**.

6. En la página **Opciones de dispositivo**, asegúrate de que la opción **Configurar la unión a Azure AD híbrido** esté seleccionada y selecciona **Siguiente**. 

7. En la página **Sistema operativo del dispositivo**, selecciona las casillas de verificación **Dispositivos unidos a un dominio de Windows 10 o posterior** y **Dispositivos unidos a un dominio de nivel inferior de Windows compatibles**, y selecciona **Siguiente**. 

   > **Nota**: los dispositivos de nivel inferior de Windows solo se admiten si usas SSO de conexión directa para dominios administrados o un servicio de federación como AD FS para dominios federados.

8. En la página **Configuración SCP**, marca la casilla del bosque **corp.contoso.com** Active Directory, selecciona la entrada **Azure Active Directory** en la lista desplegable **Servicio de autenticación** y selecciona **Agregar**.

9. Cuando se te soliciten las credenciales del administrador de empresa de corp.contoso.com, en el cuadro de diálogo **Seguridad de Windows**, inicia sesión con el nombre de usuario **CORP\\demouser** y la contraseña **demo\@pass123**.

10. De vuelta en la página **Configuración SCP**, selecciona **Siguiente**.

11. En la página **Listo para configurar**, seleccione **Configurar**.

12. En la página **Configuración completada** comprueba que la tarea se ha completado correctamente y selecciona **Salir**.


### Tarea 11: realizar una unión a Azure AD híbrido

1. En el equipo del laboratorio, en Azure Portal, comprueba que has iniciado sesión en el inquilino de Azure AD asociado a la suscripción de Azure en la que has implementado recursos en los ejercicios del laboratorio anterior a la práctica (el **Directorio por defecto**). Si no es así, selecciona el icono **Directorio + suscripción** de la barra de herramientas de Azure Portal (a la derecha del icono de **Cloud Shell**) para cambiar a ese inquilino de Azure AD. 

2. En Azure Portal, ve a la página de la máquina virtual **APP1**.

3. En la página de la máquina virtual **APP1**, conéctate a **APP1** con el Escritorio remoto. Cuando el sistema te solicite iniciar sesión, usa el nombre de usuario **AGAyers\@<custom_domain_name>** con la contraseña **demo@pass123** donde **<custom_domain_name>** representa el nombre de dominio DNS personalizado que has asignado al inquilino de Contoso de Azure AD anteriormente en este ejercicio.

4. En la sesión de Escritorio remoto de **APP1**, en la ventana **Administrador del servidor**, inicia **Programador de tareas** en **Herramientas**. 


5. En la consola **Programador de tareas**, ve a **Biblioteca de Programador de tareas** > **Microsoft** > **Windows** > **Unión de lugar de trabajo**. Desde ahí, habilita y ejecuta la tarea **Automatic-Device-Join**. 


6. Cambia a la sesión de Escritorio remoto de **DC1** y, desde el panel de la consola de la ventana de Windows PowerShell ISE, inicia la sincronización delta de Azure AD Connect ejecutando lo siguiente:

   ```pwsh
   Import-Module -Name 'C:\Program Files\Microsoft Azure AD Sync\Bin\ADSync\ADSync.psd1'
   
   Start-ADSyncSyncCycle -PolicyType Delta
   ```

7. Vuelva a la sesión de Escritorio remoto de **APP1** e inicia un **símbolo del sistema**.

8. En la ventana del símbolo del sistema, comprueba el estado de registro de Azure AD de APP1 ejecutando lo siguiente: 

   ```
   dsregcmd /status
   ```

9. Comprueba que la salida del comando se parezca a la siguiente:

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
11. Vuelve a la sesión de Escritorio remoto de **DC1**, en la ventana del explorador Edge que muestra Azure Portal, ve a la página **Dispositivos: todos los dispositivos** del inquilino de Azure AD de Contoso y compruebe que hay una entrada que representa el servidor APP1, con el **tipo de unión** establecido en **Azure AD híbrido unido**.

   > **Nota**: es posible que tengas que esperar hasta que se notifique correctamente el estado de registro de Azure AD y su objeto de Azure AD aparezca en Azure Portal.


