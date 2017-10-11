---
title: Algemene starten van de taken voor Cloudservices | Microsoft Docs
description: Enkele voorbeelden van algemene starten van de taken die u wilt uitvoeren in uw cloud services-web-rol of functie worker biedt.
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: a7095dad-1ee7-4141-bc6a-ef19a6e570f1
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: cee23da5b089b02bfc0ef10afd60f0f2272585b1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="common-cloud-service-startup-tasks"></a>Algemene taken voor Cloud-Service starten
Dit artikel vindt enkele voorbeelden van algemene starten van de taken die u wilt uitvoeren in uw cloudservice. Starten van de taken kunt u bewerkingen uitvoeren voordat een rol wordt gestart. Bewerkingen die u wilt uitvoeren bevatten voor het installeren van een onderdeel, registreren van COM-onderdelen, registersleutels instellen of starten van een langdurige proces. 

Zie [in dit artikel](cloud-services-startup-tasks.md) over de werking van starten van de taken en specifiek het maken van de vermeldingen die een taak starten definiëren.

> [!NOTE]
> Starten van de taken zijn niet van toepassing op virtuele Machines, alleen op Cloud Service-Web- en werkrollen.
> 

## <a name="define-environment-variables-before-a-role-starts"></a>Omgevingsvariabelen definiëren voordat een rol wordt gestart
Als u omgevingsvariabelen die zijn gedefinieerd voor een specifieke taak nodig hebt, gebruikt de [omgeving] -element in de [taak] element.

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


## <a name="configure-iis-startup-with-appcmdexe"></a>Starten van IIS met AppCmd.exe configureren
De [AppCmd.exe](https://technet.microsoft.com/library/jj635852.aspx) opdrachtregelprogramma kan worden gebruikt voor het beheren van IIS-instellingen bij het opstarten op Azure. *AppCmd.exe* handige, opdrachtregel toegang biedt tot configuratie-instellingen voor gebruik in het starten van de taken in Azure. Met behulp van *AppCmd.exe*, Website-instellingen kunnen worden toegevoegd, gewijzigd of verwijderd voor toepassingen en sites.

Er zijn echter enkele dingen die u moet letten in het gebruik van *AppCmd.exe* als een taak starten:

* Starten van de taken kunnen meer dan één keer worden uitgevoerd tussen opnieuw wordt opgestart. Bijvoorbeeld, wanneer een rol wordt gerecycled.
* Als een *AppCmd.exe* actie meer dan één keer wordt uitgevoerd, kan er een fout gegenereerd. Wilt u bijvoorbeeld een sectie toevoegen *Web.config* tweemaal genereren een fout.
* Starten van de taken mislukken als ze een afsluitcode dan nul retourneren of **errorlevel**. Bijvoorbeeld, wanneer *AppCmd.exe* wordt een fout gegenereerd.

Het is raadzaam om te controleren de **errorlevel** na het aanroepen *AppCmd.exe*, dit is eenvoudig te doen als u de aanroep van inpakt *AppCmd.exe* met een *.cmd* bestand. Als u een bekende detecteren **errorlevel** antwoord, u kunt deze negeren of terug doorgeven.

De waarde voor errorlevel geretourneerd door *AppCmd.exe* worden weergegeven in het bestand winerror.h en kunnen ook worden weergegeven op [MSDN](https://msdn.microsoft.com/library/windows/desktop/ms681382.aspx).

### <a name="example-of-managing-the-error-level"></a>Voorbeeld van het beheer van het foutniveau van de
In dit voorbeeld voegt u een sectie compressie en een compressie-vermelding voor JSON voor de *Web.config* bestand met de foutafhandeling en logboekregistratie.

De relevante secties van de [ServiceDefinition.csdef] bestand hier worden weergegeven, zoals instelling de [executionContext](https://msdn.microsoft.com/library/azure/gg557552.aspx#Task) kenmerk `elevated` geven *AppCmd.exe* voldoende machtigingen om te wijzigen van de instellingen in de *Web.config* bestand:

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

De *Startup.cmd* batch-bestand gebruikt *AppCmd.exe* toevoegen van een sectie compressie en een compressie-vermelding voor JSON naar de *Web.config* bestand. De verwachte **errorlevel** van 183 is ingesteld op nul met behulp van de CONTROLEER. EXE-opdrachtregelprogramma. Onverwachte errorlevels zijn StartupErrorLog.txt aangemeld.

```cmd
REM   *** Add a compression section to the Web.config file. ***
%windir%\system32\inetsrv\appcmd set config /section:urlCompression /doDynamicCompression:True /commit:apphost >> "%TEMP%\StartupLog.txt" 2>&1

REM   ERRORLEVEL 183 occurs when trying to add a section that already exists. This error is expected if this
REM   batch file were executed twice. This can occur and must be accounted for in a Azure startup
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

De tweede firewall bepaalt verbindingen tussen de virtuele machine en de processen op dat de virtuele machine. Deze firewall kan worden beheerd door de `netsh advfirewall firewall` opdrachtregelprogramma.

Azure maakt firewallregels voor de processen binnen uw rollen gestart. Bijvoorbeeld, wanneer u een service of programma start, maakt Azure automatisch de benodigde firewallregels om toe te staan of de service te communiceren met Internet. Als u een service die wordt gestart door een proces buiten uw rol (zoals een COM +-service of een Windows-taak plannen) maakt, moet u handmatig een firewallregel voor toegang tot deze service te maken. Deze firewall-regels kunnen worden gemaakt met behulp van een taak starten.

Een starten van de taak die wordt gemaakt van een firewallregel moet hebben een [executionContext][taak] van **verhoogde**. Voeg de volgende opstarttaak naar de [ServiceDefinition.csdef] bestand.

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

Als u wilt de firewallregel toevoegen, moet u de juiste `netsh advfirewall firewall` opdrachten in het batchbestand opstarten. In dit voorbeeld vereist de taak starten van de beveiliging en versleuteling voor TCP-poort 80.

```cmd
REM   Add a firewall rule in a startup task.

REM   Add an inbound rule requiring security and encryption for TCP port 80 traffic.
netsh advfirewall firewall add rule name="Require Encryption for Inbound TCP/80" protocol=TCP dir=in localport=80 security=authdynenc action=allow >> "%TEMP%\StartupLog.txt" 2>&1

REM   If an error occurred, return the errorlevel.
EXIT /B %errorlevel%
```

## <a name="block-a-specific-ip-address"></a>Een specifiek IP-adres blokkeren
U kunt de toegang van een Azure-web-rol op een reeks opgegeven IP-adressen beperken door het wijzigen van de IIS **web.config** bestand. U moet ook een opdrachtbestand waarmee wordt ontgrendeld gebruiken de **ipSecurity** sectie van de **ApplicationHost.config** bestand.

Voor het ontgrendelen van de **ipSecurity** sectie van de **ApplicationHost.config** bestand, een opdrachtbestand maken dat wordt uitgevoerd op de rol is gestart. Maak een map op het hoofdniveau van de Webrol aangeroepen **opstarten** en maak in deze map een batchbestand aangeroepen **startup.cmd**. Dit bestand toevoegen aan uw Visual Studio-project en de eigenschappen instellen op **kopie altijd** om ervoor te zorgen dat is opgenomen in het pakket.

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

Deze opdracht kunt toevoegen de **startup.cmd** bestand:

```cmd
@echo off
@echo Installing "IPv4 Address and Domain Restrictions" feature 
powershell -ExecutionPolicy Unrestricted -command "Install-WindowsFeature Web-IP-Security"
@echo Unlocking configuration for "IPv4 Address and Domain Restrictions" feature 
%windir%\system32\inetsrv\AppCmd.exe unlock config -section:system.webServer/security/ipSecurity
```

Met deze taak wordt de **startup.cmd** batchbestand worden uitgevoerd telkens als de Webrol is geïnitialiseerd, zorgt u ervoor dat de vereiste **ipSecurity** sectie is opgeheven.

Wijzig tot slot de [system.webServer sectie](http://www.iis.net/configreference/system.webserver/security/ipsecurity#005) uw Webrol **web.config** bestand toevoegen van een lijst met IP-adressen die toegang, zoals wordt weergegeven in het volgende voorbeeld:

De configuratie van dit voorbeeld **kunt** alle IP-adressen voor toegang tot de server met uitzondering van de twee gedefinieerd

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

De configuratie van dit voorbeeld **weigert** alle IP-adressen van de toegang tot de server, met uitzondering van de twee gedefinieerd.

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

## <a name="create-a-powershell-startup-task"></a>Een PowerShell starten van de taak maken
Windows PowerShell-scripts, kunnen niet worden aangeroepen rechtstreeks vanuit de [ServiceDefinition.csdef] bestand, maar ze kunnen worden aangeroepen vanuit een batchbestand opstarten.

Niet-ondertekende scripts niet wordt uitgevoerd in PowerShell (standaard). Tenzij u uw script ondertekent, moet u PowerShell als u wilt uitvoeren van niet-ondertekende scripts configureren. Niet-ondertekende scripts uitvoeren de **ExecutionPolicy** moet worden ingesteld op **onbeperkt**. De **ExecutionPolicy** instelt dat u gebruik is op basis van de versie van Windows PowerShell.

```cmd
REM   Run an unsigned PowerShell script and log the output
PowerShell -ExecutionPolicy Unrestricted .\startup.ps1 >> "%TEMP%\StartupLog.txt" 2>&1

REM   If an error occurred, return the errorlevel.
EXIT /B %errorlevel%
```

Als u een Gastbesturingssysteem dat wordt uitgevoerd, PowerShell 2.0 of 1.0 die u kunt afdwingen dat versie 2 om uit te voeren, en als niet beschikbaar is, wordt versie 1 gebruiken.

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

## <a name="create-files-in-local-storage-from-a-startup-task"></a>Maken van bestanden in lokale opslag van een taak starten
U kunt een resource voor lokale opslag gebruiken voor het opslaan van bestanden die zijn gemaakt met het starten van de taak die later worden geopend door uw toepassing.

Toevoegen voor het maken van de bron van de lokale opslag, een [LocalResources] sectie aan de [ServiceDefinition.csdef] -bestand en voeg vervolgens de [LocalStorage] onderliggend element. Geeft de bron van de lokale opslag een unieke naam en de juiste grootte voor het starten van de taak.

Als u een resource voor lokale opslag in uw taak starten, moet u een omgevingsvariabele om te verwijzen naar de lokale resource opslaglocatie maken. Vervolgens kunnen de taak starten en de toepassing bestanden lezen en schrijven naar de bron van de lokale opslag.

De relevante secties van de **ServiceDefinition.csdef** bestand worden hier weergegeven:

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

Als u bijvoorbeeld dit **Startup.cmd** batch-bestand gebruikt de **PathToStartupStorage** omgevingsvariabele voor het maken van het bestand **MyTest.txt** op de locatie van lokale opslag.

```cmd
REM   Create a simple text file.

ECHO This text will go into the MyTest.txt file which will be in the    >  "%PathToStartupStorage%\MyTest.txt"
ECHO path pointed to by the PathToStartupStorage environment variable.  >> "%PathToStartupStorage%\MyTest.txt"
ECHO The contents of the PathToStartupStorage environment variable is   >> "%PathToStartupStorage%\MyTest.txt"
ECHO "%PathToStartupStorage%".                                          >> "%PathToStartupStorage%\MyTest.txt"

REM   Exit the batch file with ERRORLEVEL 0.

EXIT /b 0
```

U kunt lokale opslagmap openen vanaf de Azure SDK met behulp van de [GetLocalResource](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) methode.

```csharp
string localStoragePath = Microsoft.WindowsAzure.ServiceRuntime.RoleEnvironment.GetLocalResource("StartupLocalStorage").RootPath;

string fileContent = System.IO.File.ReadAllText(System.IO.Path.Combine(localStoragePath, "MyTestFile.txt"));
```

## <a name="run-in-the-emulator-or-cloud"></a>Uitgevoerd in de emulator of de cloud
U kunt uw opstarttaak verschillende stappen uitvoeren wanneer deze wordt uitgevoerd in de cloud in vergelijking met wanneer het zich in de rekenemulator hebben. U wilt bijvoorbeeld een nieuw exemplaar van uw SQL-gegevens gebruiken alleen bij het uitvoeren in de emulator. Of u kunt doen prestatieoptimalisatie voor de cloud die u niet hoeft te doen wanneer uitgevoerd in de emulator.

Dit kunnen verschillende acties uitvoeren op de rekenemulator en de cloud kunt u doen met het maken van een omgevingsvariabele in de [ServiceDefinition.csdef] bestand. U test die omgevingsvariabele voor een waarde in uw taak starten.

Voor het maken van de omgevingsvariabele toevoegen de [variabele]/[RoleInstanceValue] element en het maken van een XPath-waarde van `/RoleEnvironment/Deployment/@emulated`. De waarde van de **% ComputeEmulatorRunning %** omgevingsvariabele is `true` wanneer u gebruikmaakt van de rekenemulator en `false` wanneer u gebruikmaakt van de cloud.

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

Controleer de taak kan nu de **% ComputeEmulatorRunning %** omgevingsvariabele andere acties uit te voeren op basis van de vraag of de rol wordt uitgevoerd in de cloud of de emulator. Hier volgt een cmd-shell-script waarmee wordt gecontroleerd of deze omgevingsvariabele.

```cmd
REM   Check if this task is running on the compute emulator.

IF "%ComputeEmulatorRunning%" == "true" (
    REM   This task is running on the compute emulator. Perform tasks that must be run only in the compute emulator.
) ELSE (
    REM   This task is running on the cloud. Perform tasks that must be run only in the cloud.
)
```


## <a name="detect-that-your-task-has-already-run"></a>Detecteren of de taak al is uitgevoerd
De rol kan niet opnieuw starten van de taken opnieuw uit te voeren waardoor start recyclen. Er is geen vlag die aangeeft dat een taak al is uitgevoerd op de host VM. Mogelijk hebt u een aantal taken waarbij het maakt niet uit dat ze meerdere keren worden uitgevoerd. U kunt echter uitvoeren in een situatie waarin u wilt voorkomen dat een taak meerdere keren wordt uitgevoerd.

De eenvoudigste manier om te detecteren of er al een taak is uitgevoerd, is voor het maken van een bestand in de **% TEMP %** map wanneer de taak geslaagd is en zoeken naar deze aan het begin van de taak. Hier volgt een voorbeeld cmd shell-script dat die voor u uitvoert.

```cmd
REM   If Task1_Success.txt exists, then Application 1 is already installed.
IF EXIST "%RoleRoot%\Task1_Success.txt" (
  ECHO Application 1 is already installed. Exiting. >> "%TEMP%\StartupLog.txt" 2>&1
  GOTO Finish
)

REM   Run your real exe task
ECHO Running XYZ >> "%TEMP%\StartupLog.txt" 2>&1
"%PathToApp1Install%\setup.exe" >> "%TEMP%\StartupLog.txt" 2>&1

IF %ERRORLEVEL% EQU 0 (
  REM   The application installed without error. Create a file to indicate that the task
  REM   does not need to be run again.

  ECHO This line will create a file to indicate that Application 1 installed correctly. > "%RoleRoot%\Task1_Success.txt"

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

## <a name="task-best-practices"></a>Aanbevolen procedures van taak
Hier volgen enkele aanbevolen procedures die u volgen moet wanneer de taak voor uw web- of worker-rol configureert.

### <a name="always-log-startup-activities"></a>Meld u altijd starten van activiteiten
Visual Studio biedt geen een foutopsporingsprogramma stap batchbestanden, dus is het raadzaam om zo veel mogelijk gegevens ophalen over de werking van de batch-bestanden mogelijk wilt uitvoeren. Logboekregistratie van de uitvoer van de batch-bestanden, beide **stdout** en **stderr**, krijgt u belangrijke informatie bij het opsporen en oplossen van batch-bestanden. Aan te melden beide **stdout** en **stderr** naar de StartupLog.txt bestand in de map waarnaar wordt verwezen door de **% TEMP %** omgevingsvariabele, Voeg tekst toe `>>  "%TEMP%\\StartupLog.txt" 2>&1` aan het einde van specifieke regels die u wilt registreren. Bijvoorbeeld, om het uitvoeren van setup.exe in de **% PathToApp1Install** directory:

    "%PathToApp1Install%\setup.exe" >> "%TEMP%\StartupLog.txt" 2>&1

Als u wilt uw XML-vereenvoudigen, kunt u een wrapper *cmd* bestand die alle uw opstarten aanroept taken samen met de logboekregistratie en zorgt ervoor dat elke onderliggende-taak deelt de dezelfde omgevingsvariabelen.

Wellicht vindt u het vervelend dat moet worden gebruikt `>> "%TEMP%\StartupLog.txt" 2>&1` aan het einde van elke taak starten. U kunt de taak logboekregistratie afdwingen door het maken van een wrapper die verantwoordelijk is voor logboekregistratie voor u. Deze wrapper roept het echte batchbestand dat u wilt uitvoeren. Elke uitvoer van het doelbestand voor de batch wordt omgeleid naar de *Startuplog.txt* bestand.

Het volgende voorbeeld laat zien hoe alle uitvoer omleiden van een starten van de batch-bestand. In dit voorbeeld wordt het ServerDefinition.csdef maakt een taak gestart die aanroept *logwrap.cmd*. *logwrap.cmd* aanroepen *Startup2.cmd*, alle uitvoer naar omleiden **% TEMP %\\StartupLog.txt**.

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

### <a name="set-executioncontext-appropriately-for-startup-tasks"></a>Set executionContext op de juiste wijze voor opstarttaken
Bevoegdheden voor het starten van de taak op de juiste wijze ingesteld. Soms moeten taken starten met verhoogde bevoegdheden uitvoeren, ondanks dat de rol wordt uitgevoerd met normale bevoegdheden.

De [executionContext][taak] kenmerk Hiermee stelt u de bevoegdheden van de taak starten. Met behulp van `executionContext="limited"` betekent dat het starten van de taak heeft dezelfde toegangsrechten als de rol. Met behulp van `executionContext="elevated"` betekent dat de taak starten van de administrator-bevoegdheden heeft, waardoor de opstarttaak beheerderstaken uitvoeren zonder dat de administrator-bevoegdheden voor uw rol.

Een voorbeeld van een taak gestart die zijn verhoogde bevoegdheden vereist is een taak gestart die gebruikmaakt van **AppCmd.exe** om IIS te configureren. **AppCmd.exe** vereist `executionContext="elevated"`.

### <a name="use-the-appropriate-tasktype"></a>Gebruik de juiste taskType
De [taskType][taak] kenmerk bepaalt de manier waarop het starten van de taak wordt uitgevoerd. Er zijn drie waarden: **eenvoudige**, **achtergrond**, en **voorgrond**. De taken en achtergrond asynchroon worden gestart en vervolgens de eenvoudige taken synchroon uitgevoerd één tegelijk.

Met **eenvoudige** starten van de taken, kunt u de volgorde waarin de taken worden uitgevoerd door de volgorde waarin de taken worden weergegeven in het bestand ServiceDefinition.csdef instellen. Als een **eenvoudige** taak eindigt met een afsluitcode dan nul en vervolgens de procedure opstarten stopt en de rol wordt niet gestart.

Het verschil tussen **achtergrond** starten van de taken en **voorgrond** starten van de taken is dat **voorgrond** taken houden de functie die wordt uitgevoerd totdat de  **voorgrond** taak is geëindigd. Dit betekent ook dat wanneer de **voorgrond** taak vastloopt of crasht, de functie niet gerecycled totdat de **voorgrond** taak wordt geforceerd gesloten. Om deze reden **achtergrond** taken worden aanbevolen voor het starten van de asynchrone taken alleen u deze functie van moet de **voorgrond** taak.

### <a name="end-batch-files-with-exit-b-0"></a>Einde batch-bestanden met de AFSLUITCODE /B 0
De rol wordt alleen gestart als de **errorlevel** van elk van uw eenvoudige starten van de taak is aan nul. Niet alle programma's instellen de **errorlevel** (afsluitcode) correct, dus het batchbestand moet eindigen met een `EXIT /B 0` als alles goed is uitgevoerd.

Een ontbrekende `EXIT /B 0` aan het einde van een starten van de batch-bestand is een veelvoorkomende oorzaak van de functies die niet worden gestart.

> [!NOTE]
> Ik opgevallen dat geneste batch bestanden soms vastlopen wanneer u de `/B` parameter. U kunt om ervoor te zorgen dat dit probleem blijft hangen niet gebeurt als een ander batchbestand het huidige batchbestand roept, zoals als u de [logboek wrapper](#always-log-startup-activities). U kunt weglaten de `/B` parameter in dit geval.
> 
> 

### <a name="expect-startup-tasks-to-run-more-than-once"></a>Starten van de taken uitvoeren meer dan één keer verwacht
Niet alle rol recyclet zijn opnieuw worden opgestart, maar alle rol recyclet omvatten alle starten van de taken uitgevoerd. Dit betekent dat starten van de taken kunnen moet worden meerdere keren opnieuw wordt gestart zonder problemen uitgevoerd. Dit wordt besproken de [voorgaande sectie](#detect-that-your-task-has-already-run).

### <a name="use-local-storage-to-store-files-that-must-be-accessed-in-the-role"></a>Lokale opslag gebruiken voor het opslaan van bestanden die moeten worden geopend in de rol
Als u wilt kopiëren of een bestand maken tijdens het starten van de taak die vervolgens toegankelijk is voor uw rol, moet dit bestand in de lokale opslag worden geplaatst. Zie de [voorgaande sectie](#create-files-in-local-storage-from-a-startup-task).

## <a name="next-steps"></a>Volgende stappen
Bekijk de cloud [service model- en -pakket](cloud-services-model-and-package.md)

Meer informatie over het [taken](cloud-services-startup-tasks.md) werken.

[Maken en implementeren van](cloud-services-how-to-create-deploy-portal.md) uw cloud-pakket.

[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[taak]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[Startup]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[Runtime]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[omgeving]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[variabele]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx
[Eindpunten]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Endpoints
[LocalStorage]: https://msdn.microsoft.com/library/azure/gg557552.aspx#LocalStorage
[LocalResources]: https://msdn.microsoft.com/library/azure/gg557552.aspx#LocalResources
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
