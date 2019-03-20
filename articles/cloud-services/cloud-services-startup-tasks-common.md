---
title: Veelvoorkomende opstarttaken voor Cloud Services | Microsoft Docs
description: Bevat enkele voorbeelden van veelvoorkomende opstarttaken die u wilt uitvoeren in uw cloud services-Webrol of werkrol.
services: cloud-services
documentationcenter: ''
author: jpconnock
manager: timlt
editor: ''
ms.assetid: a7095dad-1ee7-4141-bc6a-ef19a6e570f1
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: jeconnoc
ms.openlocfilehash: ec3952f2bb0b4180f5c72d948d1835a903152f0d
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58181823"
---
# <a name="common-cloud-service-startup-tasks"></a>Veelvoorkomende opstarttaken voor Cloud-Service
Dit artikel bevat enkele voorbeelden van veelvoorkomende opstarttaken die u wilt uitvoeren in uw cloudservice. U kunt opstarttaken gebruiken bewerkingen uit te voeren voordat een rol wordt gestart. Bewerkingen die u wilt uitvoeren zijn onder meer een onderdeel installeren, het registreren van COM-onderdelen, registersleutels instellen of starten van een langlopende proces. 

Zie [in dit artikel](cloud-services-startup-tasks.md) om te begrijpen hoe opstarttaken werken en informatie over het maken van de items die een opstarttaak definiëren.

> [!NOTE]
> Opstarttaken zijn niet van toepassing op virtuele Machines, alleen op Cloud Service-Web- en werkrollen.
> 

## <a name="define-environment-variables-before-a-role-starts"></a>Omgevingsvariabelen definiëren voordat een rol wordt gestart
Als u omgevingsvariabelen gedefinieerd voor een bepaalde taak nodig hebt, gebruikt u de [omgeving] -element in de [taak] element.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WorkerRole name="WorkerRole1">
        ...
        <Startup>
            <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
                <Environment>
                    <Variable name="MyEnvironmentVariable" value="MyVariableValue" />
                </Environment>
            </Task>
        </Startup>
    </WorkerRole>
</ServiceDefinition>
```

Variabelen kunt ook een [geldige Azure XPath-waarde](cloud-services-role-config-xpath.md) om te verwijzen naar iets over de implementatie. In plaats van de `value` kenmerk, het definiëren van een [RoleInstanceValue] onderliggend element.

```xml
<Variable name="PathToStartupStorage">
    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='StartupLocalStorage']/@path" />
</Variable>
```


## <a name="configure-iis-startup-with-appcmdexe"></a>Opstarten van IIS configureren met AppCmd.exe
De [AppCmd.exe](https://technet.microsoft.com/library/jj635852.aspx) opdrachtregel-hulpprogramma kan worden gebruikt voor het beheren van IIS-instellingen bij het opstarten op Azure. *AppCmd.exe* handige, opdrachtregel toegang biedt tot configuratie-instellingen voor gebruik in opstarttaken op Azure. Met behulp van *AppCmd.exe*, Website-instellingen kunnen worden toegevoegd, gewijzigd of verwijderd voor toepassingen en websites.

Er zijn echter enkele dingen Kijk uit voor het gebruik van *AppCmd.exe* als een opstarttaak:

* Opstarttaken kunnen meer dan één keer worden uitgevoerd tussen het opnieuw opstarten. Bijvoorbeeld, wanneer een rol wordt gerecycled.
* Als een *AppCmd.exe* actie meer dan één keer wordt uitgevoerd, kan er een fout gegenereerd. Bijvoorbeeld, er wordt geprobeerd om toe te voegen een sectie *Web.config* tweemaal kan een fout gegenereerd.
* Opstarttaken mislukken als ze afsluitcode dan nul retourneert of **errorlevel**. Bijvoorbeeld, wanneer *AppCmd.exe* genereert een fout.

Het is raadzaam om te controleren of de **errorlevel** na het aanroepen *AppCmd.exe*, dit is eenvoudig te doen als u de aanroep van Inpakken *AppCmd.exe* met een *.cmd* bestand. Als u een bekende detecteren **errorlevel** antwoord, u kunt deze negeren of terug doorgeven.

De errorlevel geretourneerd door *AppCmd.exe* worden weergegeven in het bestand winerror.h en kunnen ook worden weergegeven op [MSDN](https://msdn.microsoft.com/library/windows/desktop/ms681382.aspx).

### <a name="example-of-managing-the-error-level"></a>Voorbeeld van het beheer van het foutniveau van de
In dit voorbeeld voegt u een sectie compressie en een vermelding compressie voor JSON naar de *Web.config* bestand met de foutafhandeling en logboekregistratie.

De relevante secties van de [ServiceDefinition.csdef] bestand hier worden weergegeven, waaronder instelling de [executionContext](https://msdn.microsoft.com/library/azure/gg557552.aspx#Task) kenmerk `elevated` geven *AppCmd.exe* voldoende machtigingen om te wijzigen van de instellingen in de *Web.config* bestand:

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WorkerRole name="WorkerRole1">
        ...
        <Startup>
            <Task commandLine="Startup.cmd" executionContext="elevated" taskType="simple" />
        </Startup>
    </WorkerRole>
</ServiceDefinition>
```

De *Startup.cmd* batch-bestand gebruikt *AppCmd.exe* om toe te voegen een sectie compressie en een vermelding compressie voor JSON naar de *Web.config* bestand. De verwachte **errorlevel** van 183 is ingesteld op nul met behulp van de controleren. EXE-opdrachtregelprogramma. Onverwachte errorlevels worden geregistreerd in StartupErrorLog.txt.

```cmd
REM   *** Add a compression section to the Web.config file. ***
%windir%\system32\inetsrv\appcmd set config /section:urlCompression /doDynamicCompression:True /commit:apphost >> "%TEMP%\StartupLog.txt" 2>&1

REM   ERRORLEVEL 183 occurs when trying to add a section that already exists. This error is expected if this
REM   batch file were executed twice. This can occur and must be accounted for in an Azure startup
REM   task. To handle this situation, set the ERRORLEVEL to zero by using the Verify command. The Verify
REM   command will safely set the ERRORLEVEL to zero.
IF %ERRORLEVEL% EQU 183 DO VERIFY > NUL

REM   If the ERRORLEVEL is not zero at this point, some other error occurred.
IF %ERRORLEVEL% NEQ 0 (
    ECHO Error adding a compression section to the Web.config file. >> "%TEMP%\StartupLog.txt" 2>&1
    GOTO ErrorExit
)

REM   *** Add compression for json. ***
%windir%\system32\inetsrv\appcmd set config  -section:system.webServer/httpCompression /+"dynamicTypes.[mimeType='application/json; charset=utf-8',enabled='True']" /commit:apphost >> "%TEMP%\StartupLog.txt" 2>&1
IF %ERRORLEVEL% EQU 183 VERIFY > NUL
IF %ERRORLEVEL% NEQ 0 (
    ECHO Error adding the JSON compression type to the Web.config file. >> "%TEMP%\StartupLog.txt" 2>&1
    GOTO ErrorExit
)

REM   *** Exit batch file. ***
EXIT /b 0

REM   *** Log error and exit ***
:ErrorExit
REM   Report the date, time, and ERRORLEVEL of the error.
DATE /T >> "%TEMP%\StartupLog.txt" 2>&1
TIME /T >> "%TEMP%\StartupLog.txt" 2>&1
ECHO An error occurred during startup. ERRORLEVEL = %ERRORLEVEL% >> "%TEMP%\StartupLog.txt" 2>&1
EXIT %ERRORLEVEL%
```

## <a name="add-firewall-rules"></a>Firewallregels toevoegen
In Azure zijn er effectief twee firewalls. De eerste firewall bepaalt verbindingen tussen de virtuele machine en de buitenwereld. Deze firewall wordt bepaald door de [eindpunten] -element in de [ServiceDefinition.csdef] bestand.

De tweede firewall bepaalt verbindingen tussen de virtuele machine en de processen binnen die virtuele machine. Deze firewall kan worden beheerd door de `netsh advfirewall firewall` opdrachtregel-hulpprogramma.

Azure maakt een firewall-regels voor de processen die binnen uw rollen aan de slag. Bijvoorbeeld, wanneer u een service of het programma start, maakt Azure automatisch de vereiste firewallregels zodat deze kan communiceren met Internet. Als u een service die is gestart door een proces buiten uw rol (zoals een COM +-service of een geplande taak voor Windows) maakt, moet u handmatig een firewallregel voor toegang tot deze service te maken. Deze firewallregels kunnen worden gemaakt met behulp van een opstarttaak.

Een taak gestart die wordt een firewallregel gemaakt moet hebben een [executionContext][taak] van **verhoogde**. Voeg de volgende opstarttaak naar de [ServiceDefinition.csdef] bestand.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WorkerRole name="WorkerRole1">
        ...
        <Startup>
            <Task commandLine="AddFirewallRules.cmd" executionContext="elevated" taskType="simple" />
        </Startup>
    </WorkerRole>
</ServiceDefinition>
```

Als u wilt de firewallregel toevoegen, moet u de juiste `netsh advfirewall firewall` opdrachten in uw batch-bestand van het opstarten. In dit voorbeeld vereist opstarttaak van de beveiliging en versleuteling voor TCP-poort 80.

```cmd
REM   Add a firewall rule in a startup task.

REM   Add an inbound rule requiring security and encryption for TCP port 80 traffic.
netsh advfirewall firewall add rule name="Require Encryption for Inbound TCP/80" protocol=TCP dir=in localport=80 security=authdynenc action=allow >> "%TEMP%\StartupLog.txt" 2>&1

REM   If an error occurred, return the errorlevel.
EXIT /B %errorlevel%
```

## <a name="block-a-specific-ip-address"></a>Een specifiek IP-adres blokkeren
U kunt de toegang van een Azure-web-rol op een set opgegeven IP-adressen beperken door het wijzigen van de IIS **web.config** bestand. U moet ook een opdrachtbestand dat wordt ontgrendeld gebruiken de **ipSecurity** sectie van de **ApplicationHost.config** bestand.

Voor het ontgrendelen van de **ipSecurity** sectie van de **ApplicationHost.config** bestand, maakt u een opdrachtbestand dat wordt uitgevoerd aan begin van de rol. Maak een map op het hoogste niveau van uw Webrol met de naam **opstarten** en maak een batchbestand met de naam in deze map **startup.cmd**. Dit bestand toevoegen aan uw Visual Studio-project en de eigenschappen instellen op **kopie altijd** om ervoor te zorgen dat het is opgenomen in het pakket.

Voeg de volgende opstarttaak naar de [ServiceDefinition.csdef] bestand.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WebRole name="WebRole1">
        ...
        <Startup>
            <Task commandLine="startup.cmd" executionContext="elevated" />
        </Startup>
    </WebRole>
</ServiceDefinition>
```

Toevoegen van deze opdracht om de **startup.cmd** bestand:

```cmd
@echo off
@echo Installing "IPv4 Address and Domain Restrictions" feature 
powershell -ExecutionPolicy Unrestricted -command "Install-WindowsFeature Web-IP-Security"
@echo Unlocking configuration for "IPv4 Address and Domain Restrictions" feature 
%windir%\system32\inetsrv\AppCmd.exe unlock config -section:system.webServer/security/ipSecurity
```

Met deze taak wordt de **startup.cmd** batch-bestand om te worden uitgevoerd telkens als de Webrol is geïnitialiseerd, ervoor zorgen dat de vereiste **ipSecurity** sectie is ontgrendeld.

Wijzig tot slot de [system.webServer sectie](https://www.iis.net/configreference/system.webserver/security/ipsecurity#005) uw Webrol **web.config** bestand om toe te voegen een lijst met IP-adressen die toegang, zoals wordt weergegeven in het volgende voorbeeld:

Deze voorbeeld-configuratie **kunt** alle IP-adressen voor toegang tot de server met uitzondering van de twee gedefinieerd

```xml
<system.webServer>
    <security>
    <!--Unlisted IP addresses are granted access-->
    <ipSecurity>
        <!--The following IP addresses are denied access-->
        <add allowed="false" ipAddress="192.168.100.1" subnetMask="255.255.0.0" />
        <add allowed="false" ipAddress="192.168.100.2" subnetMask="255.255.0.0" />
    </ipSecurity>
    </security>
</system.webServer>
```

Deze voorbeeld-configuratie **weigert** alle IP-adressen van de toegang tot de server, met uitzondering van de twee gedefinieerd.

```xml
<system.webServer>
    <security>
    <!--Unlisted IP addresses are denied access-->
    <ipSecurity allowUnlisted="false">
        <!--The following IP addresses are granted access-->
        <add allowed="true" ipAddress="192.168.100.1" subnetMask="255.255.0.0" />
        <add allowed="true" ipAddress="192.168.100.2" subnetMask="255.255.0.0" />
    </ipSecurity>
    </security>
</system.webServer>
```

## <a name="create-a-powershell-startup-task"></a>Een opstarttaak PowerShell maken
Windows PowerShell-scripts kunnen niet worden aangeroepen rechtstreeks vanuit de [ServiceDefinition.csdef] bestand, maar ze kunnen worden aangeroepen vanuit binnen een batch-bestand van het opstarten.

PowerShell (standaard) wordt niet uitgevoerd voor niet-ondertekende scripts. Tenzij u het script zich aanmeldt, moet u PowerShell configureren voor het uitvoeren van niet-ondertekende scripts. Niet-ondertekende scripts uit te voeren de **ExecutionPolicy** moet worden ingesteld op **onbeperkt**. De **ExecutionPolicy** instelling die u hebt gebruik is gebaseerd op de versie van Windows PowerShell.

```cmd
REM   Run an unsigned PowerShell script and log the output
PowerShell -ExecutionPolicy Unrestricted .\startup.ps1 >> "%TEMP%\StartupLog.txt" 2>&1

REM   If an error occurred, return the errorlevel.
EXIT /B %errorlevel%
```

Als u een Gastbesturingssysteem dat wordt uitgevoerd, PowerShell 2.0 of 1.0 die u kunt afdwingen dat versie 2 om uit te voeren, en als niet beschikbaar is, moet u versie 1 gebruiken.

```cmd
REM   Attempt to set the execution policy by using PowerShell version 2.0 syntax.
PowerShell -Version 2.0 -ExecutionPolicy Unrestricted .\startup.ps1 >> "%TEMP%\StartupLog.txt" 2>&1

REM   If PowerShell version 2.0 isn't available. Set the execution policy by using the PowerShell
IF %ERRORLEVEL% EQU -393216 (
   PowerShell -Command "Set-ExecutionPolicy Unrestricted" >> "%TEMP%\StartupLog.txt" 2>&1
   PowerShell .\startup.ps1 >> "%TEMP%\StartupLog.txt" 2>&1
)

REM   If an error occurred, return the errorlevel.
EXIT /B %errorlevel%
```

## <a name="create-files-in-local-storage-from-a-startup-task"></a>Bestanden maken in de lokale opslag van een opstarttaak
U kunt een resource van de lokale opslag gebruiken voor het opslaan van bestanden die zijn gemaakt door de opstarttaak die later worden geopend door uw toepassing.

Voor het maken van de lokale opslag-resource toevoegen een [LocalResources] sectie aan de [ServiceDefinition.csdef] -bestand en voeg de [LocalStorage] onderliggend element. Geef de lokale opslag-resource een unieke naam en een geschikte grootte voor uw opstarttaak.

Voor het gebruik van een lokale opslag-resource in uw taak starten, moet u een omgevingsvariabele om te verwijzen naar de locatie van de resource lokale opslag maken. De taak starten en de toepassing worden vervolgens kan lezen en schrijven van bestanden naar de lokale opslag-resource.

De relevante secties van de **ServiceDefinition.csdef** bestand hier worden weergegeven:

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WorkerRole name="WorkerRole1">
    ...

    <LocalResources>
      <LocalStorage name="StartupLocalStorage" sizeInMB="5"/>
    </LocalResources>

    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>
          <Variable name="PathToStartupStorage">
            <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='StartupLocalStorage']/@path" />
          </Variable>
        </Environment>
      </Task>
    </Startup>
  </WorkerRole>
</ServiceDefinition>
```

Een voorbeeld: dit **Startup.cmd** batch-bestand gebruikt de **PathToStartupStorage** omgevingsvariabele maken van het bestand **MyTest.txt** op de locatie van de lokale opslag.

```cmd
REM   Create a simple text file.

ECHO This text will go into the MyTest.txt file which will be in the    >  "%PathToStartupStorage%\MyTest.txt"
ECHO path pointed to by the PathToStartupStorage environment variable.  >> "%PathToStartupStorage%\MyTest.txt"
ECHO The contents of the PathToStartupStorage environment variable is   >> "%PathToStartupStorage%\MyTest.txt"
ECHO "%PathToStartupStorage%".                                          >> "%PathToStartupStorage%\MyTest.txt"

REM   Exit the batch file with ERRORLEVEL 0.

EXIT /b 0
```

U kunt toegang tot lokale opslagmap van de Azure-SDK met behulp van de [GetLocalResource](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) methode.

```csharp
string localStoragePath = Microsoft.WindowsAzure.ServiceRuntime.RoleEnvironment.GetLocalResource("StartupLocalStorage").RootPath;

string fileContent = System.IO.File.ReadAllText(System.IO.Path.Combine(localStoragePath, "MyTestFile.txt"));
```

## <a name="run-in-the-emulator-or-cloud"></a>In de emulator of cloud worden uitgevoerd
U kunt de verschillende stappen uitvoeren wanneer deze wordt uitgevoerd in de cloud ten opzichte van wanneer deze zich in de rekenemulator opstarttaak hebben. U wilt bijvoorbeeld een nieuwe kopie van uw SQL-gegevens gebruiken alleen bij het uitvoeren in de emulator. Of u wilt doen prestatieoptimalisatie voor de cloud die u niet hoeft te doen wanneer in de emulator wordt uitgevoerd.

Dit kunnen verschillende acties uitvoeren op de rekenemulator en de cloud kan worden bereikt door het maken van een omgevingsvariabele in de [ServiceDefinition.csdef] bestand. U test deze omgevingsvariabele voor een waarde in de opstarttaak.

Voor het maken van de omgevingsvariabele toevoegen de [Variable]/[RoleInstanceValue] element en het maken van een XPath-waarde van `/RoleEnvironment/Deployment/@emulated`. De waarde van de **ComputeEmulatorRunning %** omgevingsvariabele is `true` wanneer u gebruikmaakt van de rekenemulator en `false` wanneer u gebruikmaakt van de cloud.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WorkerRole name="WorkerRole1">

    ...

    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>
          <Variable name="ComputeEmulatorRunning">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
        </Environment>
      </Task>
    </Startup>

  </WorkerRole>
</ServiceDefinition>
```

Controleer de taak kan nu de **ComputeEmulatorRunning %** omgevingsvariabele verschillende acties uit te voeren op basis van op of de rol in de cloud of in de emulator wordt uitgevoerd. Hier volgt een .cmd shell-script waarmee wordt gecontroleerd of voor deze omgevingsvariabele.

```cmd
REM   Check if this task is running on the compute emulator.

IF "%ComputeEmulatorRunning%" == "true" (
    REM   This task is running on the compute emulator. Perform tasks that must be run only in the compute emulator.
) ELSE (
    REM   This task is running on the cloud. Perform tasks that must be run only in the cloud.
)
```


## <a name="detect-that-your-task-has-already-run"></a>Detecteert dat de taak al uitgevoerd
De rollen kan opnieuw gebruiken zonder opnieuw opstarten waardoor uw opstarttaken opnieuw uit te voeren. Er is geen markering om aan te geven dat een taak al is uitgevoerd op de host virtuele machine. Mogelijk hebt u enkele taken waarbij het maakt niet uit dat ze meerdere keren worden uitgevoerd. U kunt echter uitvoeren in een situatie waarin u wilt voorkomen dat een taak meerdere keren wordt uitgevoerd.

De eenvoudigste manier om te detecteren dat een taak al is uitgevoerd, is het maken van een bestand in de **% TEMP %** map wanneer de taak voltooid is en zoeken naar deze aan het begin van de taak. Hier volgt een voorbeeld cmd shell-script dat dit voor u doet.

```cmd
REM   If Task1_Success.txt exists, then Application 1 is already installed.
IF EXIST "%PathToApp1Install%\Task1_Success.txt" (
  ECHO Application 1 is already installed. Exiting. >> "%TEMP%\StartupLog.txt" 2>&1
  GOTO Finish
)

REM   Run your real exe task
ECHO Running XYZ >> "%TEMP%\StartupLog.txt" 2>&1
"%PathToApp1Install%\setup.exe" >> "%TEMP%\StartupLog.txt" 2>&1

IF %ERRORLEVEL% EQU 0 (
  REM   The application installed without error. Create a file to indicate that the task
  REM   does not need to be run again.

  ECHO This line will create a file to indicate that Application 1 installed correctly. > "%PathToApp1Install%\Task1_Success.txt"

) ELSE (
  REM   An error occurred. Log the error and exit with the error code.

  DATE /T >> "%TEMP%\StartupLog.txt" 2>&1
  TIME /T >> "%TEMP%\StartupLog.txt" 2>&1
  ECHO  An error occurred running task 1. Errorlevel = %ERRORLEVEL%. >> "%TEMP%\StartupLog.txt" 2>&1

  EXIT %ERRORLEVEL%
)

:Finish

REM   Exit normally.
EXIT /B 0
```

## <a name="task-best-practices"></a>Aanbevolen procedures voor taak
Hier volgen enkele aanbevolen procedures die u volgen moet bij het configureren van de taak voor de rol van uw web- of werkrollen.

### <a name="always-log-startup-activities"></a>Meld u altijd opstarten activiteiten
Visual Studio biedt een foutopsporingsprogramma om batch-bestanden, dus is het raadzaam om zo veel gegevens ophalen over de werking van de batch-bestanden mogelijk niet. Logboekregistratie van de uitvoer van de batch-bestanden, zowel **stdout** en **stderr**, krijgt u belangrijke informatie bij het opsporen en oplossen van batch-bestanden. Om aan te melden beide **stdout** en **stderr** naar de StartupLog.txt bestand in de map waarnaar wordt verwezen door de **% TEMP %** omgevingsvariabele, Voeg tekst toe `>>  "%TEMP%\\StartupLog.txt" 2>&1` aan het einde van specifieke regels die u wilt registreren. Bijvoorbeeld, voor het uitvoeren van setup.exe in de **% PathToApp1Install** directory:

    "%PathToApp1Install%\setup.exe" >> "%TEMP%\StartupLog.txt" 2>&1

Ter vereenvoudiging van uw XML-gegevens, kunt u een wrapper *cmd* -bestand dat al uw Starter-aanroepen, samen met logboekregistratie taken en zorgt ervoor dat elke onderliggende taak deelt de dezelfde omgevingsvariabelen.

Wellicht vindt u het irritant zijn al te gebruiken `>> "%TEMP%\StartupLog.txt" 2>&1` aan het einde van elke taak starten. U kunt de taak logboekregistratie afdwingen door het maken van een wrapper die verantwoordelijk is voor logboekregistratie voor u. Deze wrapper roept de echte batch-bestand dat u wilt uitvoeren. Elke uitvoer van het doelbestand voor de batch wordt omgeleid naar de *Startuplog.txt* bestand.

Het volgende voorbeeld laat zien hoe alle uitvoer omleiden van een batch-bestand van het opstarten. In dit voorbeeld maakt u het bestand ServerDefinition.csdef een opstarttaak die worden aangeroepen *logwrap.cmd*. *logwrap.cmd* aanroepen *Startup2.cmd*, alle uitvoer omleiden **% TEMP %\\StartupLog.txt**.

ServiceDefinition.cmd:

```xml
<Startup>
    <Task commandLine="logwrap.cmd startup2.cmd" executionContext="limited" taskType="simple" />
</Startup>
```

**logwrap.cmd:**

```cmd
@ECHO OFF

REM   logwrap.cmd calls passed in batch file, redirecting all output to the StartupLog.txt log file.

ECHO [%date% %time%] == START logwrap.cmd ============================================== >> "%TEMP%\StartupLog.txt" 2>&1
ECHO [%date% %time%] Running %1 >> "%TEMP%\StartupLog.txt" 2>&1

REM   Call the child command batch file, redirecting all output to the StartupLog.txt log file.
START /B /WAIT %1 >> "%TEMP%\StartupLog.txt" 2>&1

REM   Log the completion of child command.
ECHO [%date% %time%] Done >> "%TEMP%\StartupLog.txt" 2>&1

IF %ERRORLEVEL% EQU 0 (

   REM   No errors occurred. Exit logwrap.cmd normally.
   ECHO [%date% %time%] == END logwrap.cmd ================================================ >> "%TEMP%\StartupLog.txt" 2>&1
   ECHO.  >> "%TEMP%\StartupLog.txt" 2>&1
   EXIT /B 0

) ELSE (

   REM   Log the error.
   ECHO [%date% %time%] An error occurred. The ERRORLEVEL = %ERRORLEVEL%.  >> "%TEMP%\StartupLog.txt" 2>&1
   ECHO [%date% %time%] == END logwrap.cmd ================================================ >> "%TEMP%\StartupLog.txt" 2>&1
   ECHO.  >> "%TEMP%\StartupLog.txt" 2>&1
   EXIT /B %ERRORLEVEL%

)
```

**Startup2.cmd:**

```cmd
@ECHO OFF

REM   This is the batch file where the startup steps should be performed. Because of the
REM   way Startup2.cmd was called, all commands and their outputs will be stored in the
REM   StartupLog.txt file in the directory pointed to by the TEMP environment variable.

REM   If an error occurs, the following command will pass the ERRORLEVEL back to the
REM   calling batch file.

ECHO [%date% %time%] Some log information about this task
ECHO [%date% %time%] Some more log information about this task

EXIT %ERRORLEVEL%
```

Voorbeeld van uitvoer in de **StartupLog.txt** bestand:

```txt
[Mon 10/17/2016 20:24:46.75] == START logwrap.cmd ============================================== 
[Mon 10/17/2016 20:24:46.75] Running command1.cmd 
[Mon 10/17/2016 20:24:46.77] Some log information about this task
[Mon 10/17/2016 20:24:46.77] Some more log information about this task
[Mon 10/17/2016 20:24:46.77] Done 
[Mon 10/17/2016 20:24:46.77] == END logwrap.cmd ================================================ 
```

> [!TIP]
> De **StartupLog.txt** bestand bevindt zich in de *C:\Resources\temp\\{rol-id} \RoleTemp* map.
> 
> 

### <a name="set-executioncontext-appropriately-for-startup-tasks"></a>Set-executionContext op de juiste wijze voor opstarttaken
Bevoegdheden op de juiste wijze voor de opstarttaak instellen. Soms moeten opstarttaken met verhoogde bevoegdheden uitvoeren, zelfs als de rol wordt uitgevoerd met normale bevoegdheden.

De [executionContext][taak] kenmerk Hiermee stelt u de bevoegdheden van de opstarttaak. Met behulp van `executionContext="limited"` betekent dat de opstarttaak heeft de dezelfde bevoegdheden als de rol. Met behulp van `executionContext="elevated"` betekent dat de opstarttaak heeft beheerdersbevoegdheden, waarmee de opstarttaak beheerderstaken uitvoert zonder administrator-bevoegdheden voor uw rol.

Een voorbeeld van een opstarttaak waarvoor verhoogde bevoegdheden is een opstarttaak die gebruikmaakt van **AppCmd.exe** om IIS te configureren. **AppCmd.exe** vereist `executionContext="elevated"`.

### <a name="use-the-appropriate-tasktype"></a>Gebruik de juiste taskType
De [taskType][taak] kenmerk bepaalt de manier waarop de opstarttaak wordt uitgevoerd. Er zijn drie waarden: **eenvoudige**, **achtergrond**, en **voorgrond**. De taken en achtergrond asynchroon worden gestart en vervolgens de eenvoudige taken synchroon worden uitgevoerd op een tijdstip.

Met **eenvoudige** opstarttaken, kunt u de volgorde waarin de taken worden uitgevoerd door de volgorde waarin de taken worden weergegeven in het bestand ServiceDefinition.csdef instellen. Als een **eenvoudige** taak wordt beëindigd met een afsluitcode dan nul en vervolgens de procedure opstarten stopt en de rol niet wordt gestart.

Het verschil tussen **achtergrond** opstarttaken en **voorgrond** opstarttaken is dat **voorgrond** taken houden de functie die wordt uitgevoerd totdat de  **voorgrond** taak is geëindigd. Dit betekent ook dat als de **voorgrond** taak vastloopt of crasht, de rol niet gerecycled totdat de **voorgrond** taak wordt geforceerd gesloten. Om deze reden **achtergrond** taken worden aanbevolen voor opstarten van asynchrone taken, tenzij u deze functie van moet de **voorgrond** taak.

### <a name="end-batch-files-with-exit-b-0"></a>Einde batch-bestanden met afsluiten /B 0
De rol wordt alleen gestart als de **errorlevel** van elk van de eenvoudige opstarten taak nul is. Niet alle programma's stellen de **errorlevel** (afsluitcode) correct, zodat de batch-bestand moet eindigen met een `EXIT /B 0` als alles goed is uitgevoerd.

Een ontbrekende `EXIT /B 0` aan het einde van een opstart-batch-bestand is een veelvoorkomende oorzaak van de rollen die niet worden gestart.

> [!NOTE]
> Ik opgevallen dat geneste batch bestanden soms loopt vast bij het gebruik van de `/B` parameter. Om ervoor te zorgen dat dit probleem vast te lopen niet gebeurt als een andere batchbestand uw huidige batch-bestand aanroept, zoals als u gebruik kunt u de [log-wrapper](#always-log-startup-activities). U kunt weglaten de `/B` parameter in dit geval.
> 
> 

### <a name="expect-startup-tasks-to-run-more-than-once"></a>Verwacht opstarttaken meer dan eenmaal wordt uitgevoerd
Niet alle rolrecycling bevatten een opnieuw opstarten, maar alle rolrecycling bevatten alle opstarttaken uitvoeren. Dit betekent dat opstarttaken moeten kunnen meerdere keren worden uitgevoerd tussen het opnieuw opstarten zonder problemen. Dit wordt beschreven in de [voorgaande sectie](#detect-that-your-task-has-already-run).

### <a name="use-local-storage-to-store-files-that-must-be-accessed-in-the-role"></a>Lokale opslag gebruiken voor het opslaan van bestanden die moeten worden geopend in de rol
Als u wilt kopiëren, of maak een bestand tijdens de opstarttaak die vervolgens toegankelijk is voor uw rol, moet dit bestand in de lokale opslag worden geplaatst. Zie de [voorgaande sectie](#create-files-in-local-storage-from-a-startup-task).

## <a name="next-steps"></a>Volgende stappen
Bekijk de cloud [service model en -pakket](cloud-services-model-and-package.md)

Meer informatie over hoe u [taken](cloud-services-startup-tasks.md) werken.

[Maken en implementeren van](cloud-services-how-to-create-deploy-portal.md) uw cloud service-pakket.

[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[Taak]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[Startup]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[Runtime]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[Omgeving]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[Variable]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx
[Eindpunten]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Endpoints
[LocalStorage]: https://msdn.microsoft.com/library/azure/gg557552.aspx#LocalStorage
[LocalResources]: https://msdn.microsoft.com/library/azure/gg557552.aspx#LocalResources
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
