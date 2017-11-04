---
title: .NET installeren op Azure Cloud Services-functies | Microsoft Docs
description: Dit artikel wordt beschreven hoe u .NET Framework handmatig installeren op web- en werkrollen rollen van uw cloudservice
services: cloud-services
documentationcenter: .net
author: thraka
manager: timlt
editor: 
ms.assetid: 8d1243dc-879c-4d1f-9ed0-eecd1f6a6653
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/31/2017
ms.author: adegeo
ms.openlocfilehash: cc4b62bc554757e6e394b78334f52f45aa08efe8
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="install-net-on-azure-cloud-services-roles"></a>.NET installeren op Azure Cloud Services-functies
In dit artikel beschrijft het installeren van versies van .NET Framework die niet bij de Azure-Gastbesturingssysteemreleases worden geleverd. U kunt op het Gastbesturingssysteem .NET web- en werkrollen rollen van uw cloudservice te configureren.

U kunt bijvoorbeeld .NET 4.6.1 installeren op de Gast OS-familie 4, die niet worden geleverd met een release van .NET 4.6. (De Gast OS-familie 5 maakt deel uit van .NET 4.6). Zie voor de nieuwste informatie over de Azure Gast OS-versies, de [Azure Gast OS release nieuws](cloud-services-guestos-update-matrix.md). 

>[!IMPORTANT]
>De Azure SDK 2.9 bevat een beperking van de .NET 4.6 op de Gast OS-familie 4 of eerder is geïmplementeerd. Er is een oplossing voor de beperking die beschikbaar is op de [Microsoft Docs](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9) site.

Om .NET te installeren op uw web- en werkrollen rollen, omvatten het webinstallatieprogramma voor .NET als onderdeel van uw cloudserviceproject. Het installatieprogramma starten als onderdeel van de rol starten van de taken. 

## <a name="add-the-net-installer-to-your-project"></a>Het installatieprogramma voor .NET aan uw project toevoegen
Voor het downloaden van het webinstallatieprogramma voor .NET Framework, kiest u de versie die u wilt installeren:

* [.NET 4.7.1 web-installatieprogramma](http://go.microsoft.com/fwlink/?LinkId=852095)
* [.NET 4.6.1 web-installatieprogramma](http://go.microsoft.com/fwlink/?LinkId=671729)

Toevoegen van het installatieprogramma voor een *web* rol:
  1. In **Solution Explorer**onder **rollen** in uw cloudserviceproject met de rechtermuisknop op uw *web* rol en selecteer **toevoegen**  >  **Nieuwe map**. Maak een map met de naam **bin**.
  2. Met de rechtermuisknop op de bin-map en selecteer **toevoegen** > **bestaand Item**. Het .NET-installatieprogramma selecteren en toe te voegen aan de bin-map.
  
Toevoegen van het installatieprogramma voor een *worker* rol:
* Met de rechtermuisknop op uw *worker* rol en selecteer **toevoegen** > **bestaand Item**. Het .NET-installatieprogramma selecteren en toe te voegen aan de rol. 

Wanneer bestanden worden toegevoegd op deze manier naar de inhoudsmap van de rol, zijn ze automatisch toegevoegd aan uw cloud-pakket. De bestanden zijn geïmplementeerd op een consistente locatie op de virtuele machine. Herhaal dit proces voor elke rol web- en werkrollen in uw cloudservice zodat alle rollen een kopie van het installatieprogramma hebben.

> [!NOTE]
> Zelfs als uw toepassing .NET 4.6 gericht, moet u .NET 4.6.1 installeren op uw cloud service-rol. Het Gastbesturingssysteem bevat de Knowledge Base [bijwerken 3098779](https://support.microsoft.com/kb/3098779) en [3097997 bijwerken](https://support.microsoft.com/kb/3097997). Problemen kunnen optreden tijdens het uitvoeren van uw .NET-toepassingen als .NET 4.6 is geïnstalleerd op de Knowledge Base-updates. Om te voorkomen dat deze problemen, installeert u .NET 4.6.1 in plaats van versie 4.6. Zie voor meer informatie de [Knowledge Base-artikel 3118750](https://support.microsoft.com/kb/3118750).
> 
> 

![Inhoud van de rol met installer-bestanden][1]

## <a name="define-startup-tasks-for-your-roles"></a>Starten van de taken voor uw rollen definiëren
Starten van de taken kunt u bewerkingen uitvoeren voordat een rol wordt gestart. Installatie van .NET Framework als onderdeel van de opstarttaak zorgt ervoor dat het framework is geïnstalleerd voordat een toepassingscode wordt uitgevoerd. Zie voor meer informatie over het starten van de taken, [starten van de taken uitvoeren in Azure](cloud-services-startup-tasks.md). 

1. De volgende inhoud toevoegen aan het bestand ServiceDefinition.csdef onder de **WebRole** of **WorkerRole** knooppunt voor alle rollen:
   
    ```xml
    <LocalResources>
      <LocalStorage name="NETFXInstall" sizeInMB="1024" cleanOnRoleRecycle="false" />
    </LocalResources>    
    <Startup>
      <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
        <Environment>
          <Variable name="PathToNETFXInstall">
            <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='NETFXInstall']/@path" />
          </Variable>
          <Variable name="ComputeEmulatorRunning">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
        </Environment>
      </Task>
    </Startup>
    ```
   
    De console-opdracht wordt uitgevoerd in de configuratie van de voorgaande `install.cmd` met administrator-bevoegdheden voor het installeren van .NET Framework. De configuratie maakt ook een **LocalStorage** element met de naam **NETFXInstall**. Het opstartscript Hiermee stelt u de map temp deze bron lokale opslag gebruiken. 
    
    > [!IMPORTANT]
    > Als u wilt correcte installatie van het framework, stelt u de grootte van deze resource op ten minste 1024 MB.
    
    Zie voor meer informatie over het starten van de taken [algemene Azure Cloud Services starten van de taken](cloud-services-startup-tasks-common.md).

2. Maak een bestand met de naam **install.cmd** en voeg het volgende script naar het bestand installeren.

    Het script wordt gecontroleerd of de opgegeven versie van .NET Framework is al geïnstalleerd op de computer door het opvragen van het register. Als u de .NET-versie niet is geïnstalleerd, wordt het installatieprogramma van .NET web geopend. Voor het oplossen van problemen, registreert het script alle activiteiten in het bestand startuptasklog-(de huidige datum en tijd) .txt die is opgeslagen in **InstallLogs** lokale opslag.
  
    > [!IMPORTANT]
    > Gebruik een elementaire teksteditor zoals Kladblok Windows het install.cmd-bestand te maken. Als u Visual Studio gebruikt voor het maken van een tekstbestand en wijzigt u de extensie in .cmd, kan het bestand nog steeds een UTF-8-bytevolgordemarkering bevatten. Deze is ingeschakeld, kan een fout veroorzaken wanneer de eerste regel van het script wordt uitgevoerd. Om te voorkomen dat deze fout, moet u de eerste regel van het script een een-instructie die door de verwerking van de volgorde byte worden overgeslagen. 
    > 
    >
   
    ```cmd
    REM Set the value of netfx to install appropriate .NET Framework. 
    REM ***** To install .NET 4.5.2 set the variable netfx to "NDP452" *****
    REM ***** To install .NET 4.6 set the variable netfx to "NDP46" *****
    REM ***** To install .NET 4.6.1 set the variable netfx to "NDP461" *****
    REM ***** To install .NET 4.6.2 set the variable netfx to "NDP462" *****
    REM ***** To install .NET 4.7 set the variable netfx to "NDP47" *****
    REM ***** To install .NET 4.7.1 set the variable netfx to "NDP47" *****
    set netfx="NDP471"
    
    REM ***** Set script start timestamp *****
    set timehour=%time:~0,2%
    set timestamp=%date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2%
    set "log=install.cmd started %timestamp%."
    
    REM ***** Exit script if running in Emulator *****
    if %ComputeEmulatorRunning%=="true" goto exit
    
    REM ***** Needed to correctly install .NET 4.6.1, otherwise you may see an out of disk space error *****
    set TMP=%PathToNETFXInstall%
    set TEMP=%PathToNETFXInstall%
    
    REM ***** Setup .NET filenames and registry keys *****
    if %netfx%=="NDP471" goto NDP471
    if %netfx%=="NDP47" goto NDP47
    if %netfx%=="NDP462" goto NDP462
    if %netfx%=="NDP461" goto NDP461
    if %netfx%=="NDP46" goto NDP46
        set "netfxinstallfile=NDP452-KB2901954-Web.exe"
        set netfxregkey="0x5cbf5"
        goto logtimestamp
    
    :NDP46
    set "netfxinstallfile=NDP46-KB3045560-Web.exe"
    set netfxregkey="0x6004f"
    goto logtimestamp
    
    :NDP461
    set "netfxinstallfile=NDP461-KB3102438-Web.exe"
    set netfxregkey="0x6040e"
    goto logtimestamp
    
    :NDP462
    set "netfxinstallfile=NDP462-KB3151802-Web.exe"
    set netfxregkey="0x60632"
    goto logtimestamp
    
    :NPD47
    set "netfxinstallfile=NDP47-KB3186500-Web.exe"
    set netfxregkey="0x707FE"
    goto logtimestamp
    
    :NDP471
    set "netfxinstallfile=NDP471-KB4033344-Web.exe"
    set netfxregkey="0x709fc"
    goto logtimestamp
    
    :logtimestamp
    REM ***** Setup LogFile with timestamp *****
    md "%PathToNETFXInstall%\log"
    set startuptasklog="%PathToNETFXInstall%log\startuptasklog-%timestamp%.txt"
    set netfxinstallerlog="%PathToNETFXInstall%log\NetFXInstallerLog-%timestamp%"
    echo %log% >> %startuptasklog%
    echo Logfile generated at: %startuptasklog% >> %startuptasklog%
    echo TMP set to: %TMP% >> %startuptasklog%
    echo TEMP set to: %TEMP% >> %startuptasklog%
    
    REM ***** Check if .NET is installed *****
    echo Checking if .NET (%netfx%) is installed >> %startuptasklog%
    set /A netfxregkeydecimal=%netfxregkey%
    set foundkey=0
    FOR /F "usebackq skip=2 tokens=1,2*" %%A in (`reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full" /v Release 2^>nul`) do @set /A foundkey=%%C
    echo Minimum required key: %netfxregkeydecimal% -- found key: %foundkey% >> %startuptasklog%
    if %foundkey% GEQ %netfxregkeydecimal% goto installed
    
    REM ***** Installing .NET *****
    echo Installing .NET with commandline: start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog%  /chainingpackage "CloudService Startup Task" >> %startuptasklog%
    start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog% /chainingpackage "CloudService Startup Task" >> %startuptasklog% 2>>&1
    if %ERRORLEVEL%== 0 goto installed
        echo .NET installer exited with code %ERRORLEVEL% >> %startuptasklog%    
        if %ERRORLEVEL%== 3010 goto restart
        if %ERRORLEVEL%== 1641 goto restart
        echo .NET (%netfx%) install failed with Error Code %ERRORLEVEL%. Further logs can be found in %netfxinstallerlog% >> %startuptasklog%
    
    :restart
    echo Restarting to complete .NET (%netfx%) installation >> %startuptasklog%
    EXIT /B %ERRORLEVEL%
    
    :installed
    echo .NET (%netfx%) is installed >> %startuptasklog%
    
    :end
    echo install.cmd completed: %date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2% >> %startuptasklog%
    
    :exit
    EXIT /B 0
    ```

3. Het bestand install.cmd toevoegen aan elke rol met behulp van **toevoegen** > **bestaand Item** in **Solution Explorer** zoals eerder in dit onderwerp beschreven. 

    Nadat deze stap voltooid is, moeten alle rollen van het .NET-installatiebestand en het bestand install.cmd hebben.

   ![Inhoud van de rol met alle bestanden][2]

## <a name="configure-diagnostics-to-transfer-startup-logs-to-blob-storage"></a>Diagnostische functies voor het starten van de logboeken over te dragen naar Blob-opslag configureren
Als u wilt de het oplossen van installatieproblemen vereenvoudigen, kunt u Azure Diagnostics om over te dragen van alle logboekbestanden zijn gegenereerd door het opstartscript of het installatieprogramma .NET naar Azure Blob-opslag kunt configureren. Met deze benadering kunt u de logboeken bekijken door het downloaden van de logboekbestanden van Blob-opslag in plaats van dat extern bureaublad in de rol.

Diagnostische gegevens configureren, opent u het bestand diagnostics.wadcfgx en voegt u de volgende inhoud onder de **mappen** knooppunt: 

```xml 
<DataSources>
 <DirectoryConfiguration containerName="netfx-install">
  <LocalResource name="NETFXInstall" relativePath="log"/>
 </DirectoryConfiguration>
</DataSources>
```

Deze XML configureert u diagnostische gegevens om over te dragen van de bestanden in de logboekmap in de **NETFXInstall** resource toe aan het opslagaccount voor diagnostische gegevens in de **netfx-Installeer** blob-container.

## <a name="deploy-your-cloud-service"></a>Uw cloudservice implementeren
Wanneer u uw cloudservice implementeert, installeer de taken starten van de .NET Framework als deze nog niet is geïnstalleerd. Uw cloud service-rollen zich in de *bezet* status terwijl het framework wordt geïnstalleerd. Als de framework-installatie opnieuw opstarten vereist, de functies van de service mogelijk ook opnieuw wordt opgestart. 

## <a name="additional-resources"></a>Aanvullende bronnen
* [Installatie van .NET Framework][Installing the .NET Framework]
* [Bepalen welke versies van .NET Framework zijn geïnstalleerd][How to: Determine Which .NET Framework Versions Are Installed]
* [Het oplossen van de installatie van .NET Framework][Troubleshooting .NET Framework Installations]

[How to: Determine Which .NET Framework Versions Are Installed]: /dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed
[Installing the .NET Framework]: /dotnet/framework/install/guide-for-developers
[Troubleshooting .NET Framework Installations]: /dotnet/framework/install/troubleshoot-blocked-installations-and-uninstallations

<!--Image references-->
[1]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithinstallerfiles.png
[2]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithallfiles.png
