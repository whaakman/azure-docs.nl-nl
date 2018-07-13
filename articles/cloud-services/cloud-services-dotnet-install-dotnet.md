---
title: Installeer .NET op Azure Cloud Services-rollen | Microsoft Docs
description: In dit artikel wordt beschreven hoe u .NET Framework handmatig installeren op uw web- en werkrollen cloudservicerollen
services: cloud-services
documentationcenter: .net
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 8d1243dc-879c-4d1f-9ed0-eecd1f6a6653
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/22/2018
ms.author: jeconnoc
ms.openlocfilehash: 80f525f3c7b71c2d5d1fdbd415c298452ab75423
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39004826"
---
# <a name="install-net-on-azure-cloud-services-roles"></a>Installeer .NET op Azure Cloud Services-rollen
Dit artikel wordt beschreven hoe u versies van .NET Framework die niet geleverd met de Azure-Gastbesturingssysteem te installeren. U kunt .NET op het Gastbesturingssysteem gebruiken voor het configureren van uw web- en werkrollen cloudservicerollen.

U kunt bijvoorbeeld .NET 4.6.2 installeren op de Guest OS family 4, deze worden niet geleverd met een versie van .NET 4.6. (De Guest OS family 5 maakt deel uit van .NET 4.6). Zie voor de meest recente informatie over de versies van de Azure-Gastbesturingssysteem, de [releasenieuws voor Azure-Gastbesturingssysteem](cloud-services-guestos-update-matrix.md). 

>[!IMPORTANT]
>De Azure SDK 2.9 gebruikt, bevat een beperking op .NET 4.6 implementeren op de Guest OS family 4 of eerder. Een oplossing voor de beperking is beschikbaar op de [Microsoft-Docs](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9) site.

Voor het installeren van .NET voor uw web- en werkrollen, omvatten het webinstallatieprogramma voor .NET als onderdeel van uw cloud service-project. Start het installatieprogramma als onderdeel van opstarttaken van de rol. 

## <a name="add-the-net-installer-to-your-project"></a>Het installatieprogramma voor .NET aan uw project toevoegen
Kies voor het downloaden van het webinstallatieprogramma voor .NET Framework, de versie die u wilt installeren:

* [.NET 4.7.2 web-installatieprogramma](http://go.microsoft.com/fwlink/?LinkId=863262)
* [.NET 4.6.2 web-installatieprogramma](https://www.microsoft.com/download/details.aspx?id=53345)

Toevoegen van het installatieprogramma voor een *web* rol:
  1. In **Solution Explorer**onder **rollen** in uw cloud service-project met de rechtermuisknop op uw *web* rol en selecteer **toevoegen**  >  **Nieuwe map**. Maak een map met de naam **bin**.
  2. Met de rechtermuisknop op de bin-map en selecteer **toevoegen** > **bestaand Item**. Selecteer het installatieprogramma voor .NET en toe te voegen aan de bin-map.
  
Toevoegen van het installatieprogramma voor een *worker* rol:
* Met de rechtermuisknop op uw *worker* rol en selecteer **toevoegen** > **bestaand Item**. Selecteer het installatieprogramma voor .NET en toe te voegen aan de rol. 

Wanneer bestanden worden toegevoegd op deze manier naar de inhoudsmap van de rol, worden ze automatisch toegevoegd aan uw cloud service-pakket. De bestanden zijn geïmplementeerd op een consistente locatie op de virtuele machine. Herhaal dit proces voor elke web- en werkrollen rol in uw cloudservice, zodat alle rollen beschikken over een kopie van het installatieprogramma.

> [!NOTE]
> U moet .NET 4.6.2 installeren op uw cloud service-rol, zelfs als uw toepassing is gericht op .NET 4.6. De Gast-OS omvat de Knowledge Base [bijwerken 3098779](https://support.microsoft.com/kb/3098779) en [3097997 bijwerken](https://support.microsoft.com/kb/3097997). Wanneer u uw .NET-toepassingen worden uitgevoerd als .NET 4.6 boven op de Knowledge Base-updates is geïnstalleerd, kunnen er problemen optreden. Om te voorkomen dat deze problemen, installeert u .NET 4.6.2 in plaats van versie 4.6. Zie voor meer informatie de [Knowledge Base-artikel 3118750](https://support.microsoft.com/kb/3118750) en [4340191](https://support.microsoft.com/kb/4340191).
> 
> 

![Inhoud van de rol met installer-bestanden][1]

## <a name="define-startup-tasks-for-your-roles"></a>Opstarttaken voor uw rollen definiëren
U kunt opstarttaken gebruiken bewerkingen uit te voeren voordat een rol wordt gestart. Installatie van .NET Framework als onderdeel van de opstarttaak zorgt ervoor dat het framework is geïnstalleerd voordat de code van een toepassing wordt uitgevoerd. Zie voor meer informatie over opstarttaken [opstarttaken uitvoeren in Azure](cloud-services-startup-tasks.md). 

1. Voeg de volgende inhoud toe aan het bestand ServiceDefinition.csdef onder de **WebRole** of **WorkerRole** knooppunt voor alle rollen:
   
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
   
    De consoleopdracht wordt uitgevoerd in de vorige configuratie `install.cmd` met administrator-bevoegdheden voor het installeren van het .NET Framework. De configuratie maakt ook een **LocalStorage** element met de naam **NETFXInstall**. Het opstartscript Hiermee stelt u de map temp om deze resource lokale opslag te gebruiken. 
    
    > [!IMPORTANT]
    > Om ervoor te zorgen correcte installatie van het framework, moet u de grootte van deze resource instellen op ten minste 1024 MB.
    
    Zie voor meer informatie over opstarttaken [algemene Azure Cloud Services-opstarttaken](cloud-services-startup-tasks-common.md).

2. Maak een bestand met de naam **install.cmd** en voeg het volgende script om het bestand te installeren.

   Het script wordt gecontroleerd of de opgegeven versie van .NET Framework is al geïnstalleerd op de computer door het opvragen van het register. Als de .NET-versie niet is geïnstalleerd, is klikt u vervolgens het webinstallatieprogramma voor .NET geopend. Om u te helpen bij het oplossen van problemen, legt het script alle activiteiten op het bestand startuptasklog-(huidige datum en tijd) .txt die zijn opgeslagen in **InstallLogs** lokale opslag.
   
   > [!IMPORTANT]
   > Gebruik een eenvoudige teksteditor zoals Kladblok in Windows om de install.cmd-bestand te maken. Als u Visual Studio gebruiken om te maken van een tekstbestand en wijzigt u de extensie in .cmd, kan het bestand nog steeds een UTF-8-byte-volgordemarkering bevatten. Dit is ingeschakeld kan een fout veroorzaken wanneer de eerste regel van het script wordt uitgevoerd. Om te voorkomen dat deze fout, moet u de eerste regel van het script een een-instructie die door de bestellingsverwerking byte worden overgeslagen. 
   > 
   >
   
   ```cmd
   REM Set the value of netfx to install appropriate .NET Framework. 
   REM ***** To install .NET 4.5.2 set the variable netfx to "NDP452" *****
   REM ***** To install .NET 4.6 set the variable netfx to "NDP46" *****
   REM ***** To install .NET 4.6.1 set the variable netfx to "NDP461" ***** http://go.microsoft.com/fwlink/?LinkId=671729
   REM ***** To install .NET 4.6.2 set the variable netfx to "NDP462" ***** https://www.microsoft.com/download/details.aspx?id=53345
   REM ***** To install .NET 4.7 set the variable netfx to "NDP47" ***** 
   REM ***** To install .NET 4.7.1 set the variable netfx to "NDP471" ***** http://go.microsoft.com/fwlink/?LinkId=852095
   REM ***** To install .NET 4.7.2 set the variable netfx to "NDP472" ***** http://go.microsoft.com/fwlink/?LinkId=863262
   set netfx="NDP472"
   
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
   if %netfx%=="NDP472" goto NDP472
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
   
   :NDP47
   set "netfxinstallfile=NDP47-KB3186500-Web.exe"
   set netfxregkey="0x707FE"
   goto logtimestamp
   
   :NDP471
   set "netfxinstallfile=NDP471-KB4033344-Web.exe"
   set netfxregkey="0x709fc"
   goto logtimestamp
   
   :NDP472
   set "netfxinstallfile=NDP472-KB4054531-Web.exe"
   set netfxregkey="0x70BF6"
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

    Nadat deze stap voltooid is, moeten alle rollen de installer-bestand voor .NET en het bestand install.cmd hebben.

   ![Rol van inhoud met alle bestanden][2]

## <a name="configure-diagnostics-to-transfer-startup-logs-to-blob-storage"></a>Diagnostische gegevens om over te dragen opstarten logboeken naar Blob-opslag configureren
U kunt Azure Diagnostics om over te dragen van alle logboekbestanden die worden gegenereerd door het opstartscript of het installatieprogramma .NET naar Azure Blob-opslag configureren ter vereenvoudiging van de van het oplossen van installatieproblemen. Met behulp van deze benadering kunt kunt u de logboeken weergeven door het downloaden van de logboekbestanden van Blob-opslag in plaats van dat met extern bureaublad in de rol.


Diagnostische gegevens configureren, opent u het bestand diagnostics.wadcfgx en voeg de volgende inhoud onder de **mappen** knooppunt: 

```xml 
<DataSources>
 <DirectoryConfiguration containerName="netfx-install">
  <LocalResource name="NETFXInstall" relativePath="log"/>
 </DirectoryConfiguration>
</DataSources>
```

Deze XML configureert u Diagnostics om over te dragen van de bestanden in de logboekmap in de **NETFXInstall** resource naar het opslagaccount voor diagnostische gegevens in de **netfx-installatie** blob-container.

## <a name="deploy-your-cloud-service"></a>Uw cloudservice implementeren
Wanneer u uw cloudservice implementeert, installeer de opstarttaken .NET Framework als deze nog niet geïnstalleerd. Uw cloud service-functies zijn de *bezet* status terwijl het framework wordt geïnstalleerd. Als de framework-installatie opnieuw worden opgestart moet, kunnen ook de functies van de service opnieuw. 

## <a name="additional-resources"></a>Aanvullende resources
* [Installatie van .NET Framework][Installing the .NET Framework]
* [Bepalen welke versies van .NET Framework zijn geïnstalleerd][How to: Determine Which .NET Framework Versions Are Installed]
* [Oplossen van problemen met installatie van .NET Framework][Troubleshooting .NET Framework Installations]

[How to: Determine Which .NET Framework Versions Are Installed]: /dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed
[Installing the .NET Framework]: /dotnet/framework/install/guide-for-developers
[Troubleshooting .NET Framework Installations]: /dotnet/framework/install/troubleshoot-blocked-installations-and-uninstallations

<!--Image references-->
[1]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithinstallerfiles.png
[2]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithallfiles.png
