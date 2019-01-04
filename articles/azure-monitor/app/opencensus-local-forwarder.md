---
title: Azure Application Insights OpenCensus gedistribueerde tracering lokale doorstuurserver | Microsoft docs
description: OpenCensus gedistribueerd traceringen en reeksen van talen zoals Python en Go doorsturen naar Azure Application Insights
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 09/18/2018
ms.reviewer: nimolnar
ms.author: mbullwin
ms.openlocfilehash: f5f42ae5068440c3a90bf3b374238e18781b9770
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "54003372"
---
# <a name="local-forwarder"></a>Lokale doorstuurserver

Lokale doorstuurserver is een agent die Application Insights verzamelt of [OpenCensus](https://opencensus.io/) telemetrie van een aantal SDK's en doorgestuurd naar Application Insights. Het is geschikt voor het uitvoeren onder Windows en Linux. Kunt u mogelijk ook deze uitvoeren met Mac OS, maar die wordt niet officieel ondersteund op dit moment.

## <a name="running-local-forwarder"></a>Lokale-doorstuurserver wordt uitgevoerd

Lokale doorstuurserver is een [open-SourceProject op GitHub](https://github.com/Microsoft/ApplicationInsights-LocalForwarder/releases). Er zijn tal van manieren om lokale doorstuurserver op meerdere platforms worden uitgevoerd.

### <a name="windows"></a>Windows

#### <a name="windows-service"></a>Windows-service

De eenvoudigste manier om lokale doorstuurserver die wordt uitgevoerd onder Windows is door het te installeren als een Windows-Service. De versie wordt geleverd met een Windows-Service kan worden uitgevoerd (*WindowsServiceHost/Microsoft.LocalForwarder.WindowsServiceHost.exe*) die eenvoudig kunnen worden geregistreerd met het besturingssysteem.

> [!NOTE]
> De lokale doorstuurserver minimaal .NET Framework 4.7 is vereist. Als u geen .NET Framework 4.7 de service wordt niet installeren, maar het gestart. Voor toegang tot de meest recente versie van .NET Framework **[gaat u naar de downloadpagina van .NET Framework](
https://www.microsoft.com/net/download/dotnet-framework-runtime/net472?utm_source=getdotnet&utm_medium=referral)**.

1. Download de LF. WindowsServiceHost.zip-bestand van de [lokale doorstuurserver release pagina](https://github.com/Microsoft/ApplicationInsights-LocalForwarder/releases) op GitHub.

    ![Schermafbeelding van de lokale doorstuurserver release-downloadpagina](./media/opencensus-local-forwarder/001-local-forwarder-windows-service-host-zip.png)

2. In dit voorbeeld voor een eenvoudige demonstratie we alleen het ZIP-bestand naar het pad wordt uitgepakt `C:\LF-WindowsServiceHost`.

    De service registreren en configureert om te beginnen bij het opstarten vanaf de opdrachtregel als beheerder voert u het volgende:

    ```
    sc create "Local Forwarder" binpath="C:\LF-WindowsServiceHost\Microsoft.LocalForwarder.WindowsServiceHost.exe" start=auto
    ```
    
    U ontvangt een reactie van:
    
    `[SC] CreateService SUCCESS`
    
    Het onderzoeken van een nieuwe service via de Services GUI-type ``services.msc``
        
     ![Schermopname van het lokale-doorstuurserver](./media/opencensus-local-forwarder/002-services.png)

3. **Met de rechtermuisknop op** de nieuwe lokale doorstuurserver en selecteer **Start**. Uw service voert nu een status running doorbrengt.

4. De service wordt standaard gemaakt zonder een herstelacties. U kunt **met de rechtermuisknop op** en selecteer **eigenschappen** > **Recovery** het configureren van automatische antwoorden op een service optreedt.

    Of als u liever automatische herstelopties instellen via een programma voor wanneer er fouten optreden, kunt u gebruiken:

    ```
    sc failure "Local Forwarder" reset= 432000 actions= restart/1000/restart/1000/restart/1000
    ```

5. Op dezelfde locatie bevinden als uw ``Microsoft.LocalForwarder.WindowsServiceHost.exe`` -bestand, dat in dit voorbeeld is ``C:\LF-WindowsServiceHost`` er is een bestand met de naam ``LocalForwarder.config``. Dit is een op basis van xml-bestand waarmee u de configuratie van uw localforwader aanpassen en geeft u de instrumentatiesleutel van de Application Insights-resource die u wilt dat uw gedistribueerde traceringsgegevens die worden doorgestuurd. 

    Na het bewerken van de ``LocalForwarder.config`` -bestand naar de instrumentatiesleutel toevoegen, moet u opnieuw de **lokale doorstuurserver** om toe te staan uw wijzigingen worden doorgevoerd.
    
6. Om te bevestigen dat de gewenste instellingen voldaan is en dat de lokale doorstuurserver voor traceergegevens als verwachte selectievakje luistert de ``LocalForwarder.log`` bestand. Hier ziet u resultaten die vergelijkbaar is met de afbeelding hieronder aan de onderkant van het bestand:

    ![Schermafbeelding van LocalForwarder.log bestand](./media/opencensus-local-forwarder/003-log-file.png)

#### <a name="console-application"></a>Consoletoepassing

Voor bepaalde use-cases, is het mogelijk nuttig is om lokale doorstuurserver worden uitgevoerd als een consoletoepassing. De versie wordt geleverd met de volgende uitvoerbare versies van de consolehost:
* een .NET Core framework-afhankelijke binaire */ConsoleHost/publish/Microsoft.LocalForwarder.ConsoleHost.dll*. Het uitvoeren van deze binaire waarde is vereist een .NET Core runtime om te worden geïnstalleerd. verwijzen naar deze download [pagina](https://www.microsoft.com/net/download/dotnet-core/2.1) voor meer informatie.
```batchfile
E:\uncdrop\ConsoleHost\publish>dotnet Microsoft.LocalForwarder.ConsoleHost.dll
```
* een op zichzelf staand .NET Core set van binaire bestanden voor x86 en x64-platform. Hiervoor geen .NET Core runtime om uit te voeren. */ConsoleHost/Win-x86/Publish/Microsoft.LocalForwarder.ConsoleHost.exe*, */ConsoleHost/win-x64/publish/Microsoft.LocalForwarder.ConsoleHost.exe*.
```batchfile
E:\uncdrop\ConsoleHost\win-x86\publish>Microsoft.LocalForwarder.ConsoleHost.exe
E:\uncdrop\ConsoleHost\win-x64\publish>Microsoft.LocalForwarder.ConsoleHost.exe
```

### <a name="linux"></a>Linux

Net als bij Windows, is de versie wordt geleverd met de volgende uitvoerbare versies van de consolehost:
* een .NET Core framework-afhankelijke binaire */ConsoleHost/publish/Microsoft.LocalForwarder.ConsoleHost.dll*. Het uitvoeren van deze binaire waarde is vereist een .NET Core runtime om te worden geïnstalleerd. verwijzen naar deze download [pagina](https://www.microsoft.com/net/download/dotnet-core/2.1) voor meer informatie.

```batchfile
dotnet Microsoft.LocalForwarder.ConsoleHost.dll
```

* een op zichzelf staand .NET Core instellen van de binaire bestanden voor linux-64. Deze zijn vereist om .NET Core runtime om uit te voeren. */ConsoleHost/Linux-x64/Publish/Microsoft.LocalForwarder.ConsoleHost*.

```batchfile
user@machine:~/ConsoleHost/linux-x64/publish$ sudo chmod +x Microsoft.LocalForwarder.ConsoleHost
user@machine:~/ConsoleHost/linux-x64/publish$ ./Microsoft.LocalForwarder.ConsoleHost
```

Veel Linux-gebruikers wilt lokale doorstuurserver als een daemon uitvoeren. Linux-systemen worden geleverd met tal van oplossingen voor beheer van de service, zoals Upstart, sysv of systemd. Ongeacht de specifieke versie is, kunt u deze lokale doorstuurserver uitvoeren op een manier die het meest geschikt is voor uw scenario.

Als voorbeeld gaan we een daemon-service maken met systemd. We gebruiken de afhankelijk zijn van het framework-versie, maar dezelfde kan worden gedaan voor een op zichzelf staand ook.

* de volgende service-bestand met de naam *localforwarder.service* en plaats deze in */lib/systemd/system*.
In dit voorbeeld wordt ervan uitgegaan dat uw gebruikersnaam SAMPLE_USER is en u hebt lokale doorstuurserver afhankelijk zijn van het framework binaire bestanden gekopieerd (van */ConsoleHost/publiceren*) naar */home/SAMPLE_USER/LOCALFORWARDER_DIR*.

```
# localforwarder.service
# Place this file into /lib/systemd/system/
# Use 'systemctl enable localforwarder' to start the service automatically on each boot
# Use 'systemctl start localforwarder' to start the service immediately

[Unit]
Description=Local Forwarder service
After=network.target
StartLimitIntervalSec=0

[Service]
Type=simple
Restart=always
RestartSec=1
User=SAMPLE_USER
WorkingDirectory=/home/SAMPLE_USER/LOCALFORWARDER_DIR
ExecStart=/usr/bin/env dotnet /home/SAMPLE_USER/LOCALFORWARDER_DIR/Microsoft.LocalForwarder.ConsoleHost.dll noninteractive

[Install]
WantedBy=multi-user.target
```

* Voer de volgende opdracht om te instrueren systemd lokale doorstuurserver starten op elke keer opstarten

```
systemctl enable localforwarder
```

* Voer de volgende opdracht om te instrueren systemd lokale doorstuurserver onmiddellijk starten

```
systemctl start localforwarder
```

* De service controleren door te inspecteren **.log* bestanden in de map /home/SAMPLE_USER/LOCALFORWARDER_DIR.

### <a name="mac"></a>Mac
Lokale doorstuurserver kan ook werken met Mac OS, maar het is momenteel niet officieel ondersteund.

### <a name="self-hosting"></a>Zelf die als host fungeert
Lokale doorstuurserver is ook gedistribueerd als een standaard .NET-NuGet-pakket, zodat u deze in uw eigen .NET-toepassing te hosten.

```C#
using Library;
...
Host host = new Host();

// see section below on configuring local forwarder
string configuration = ...;
    
host.Run(config, TimeSpan.FromSeconds(5));
...
host.Stop();
```

## <a name="configuring-local-forwarder"></a>Lokale doorstuurserver configureren

* Bij het uitvoeren van een van de lokale van doorstuurserver eigen hosts (Consolehost of Host voor Windows-Service), vindt u **LocalForwarder.config** geplaatst naast het binaire bestand.
* Als u zelf de lokale doorstuurserver NuGet host, de configuratie van dezelfde indeling moet worden opgegeven in de code (Zie de sectie op zelf die als host fungeert). Controleer voor de syntaxis van de configuratie van de [LocalForwarder.config](https://github.com/Microsoft/ApplicationInsights-LocalForwarder/blob/master/src/ConsoleHost/LocalForwarder.config) in de GitHub-opslagplaats. 

> [!NOTE]
> Configuratie kan wijzigen release release, dus let op welke versie u gebruikt.

## <a name="monitoring-local-forwarder"></a>Bewaking van lokale doorstuurserver

Traceringen worden geschreven naar het bestandssysteem naast het uitvoerbare bestand dat lokale-doorstuurserver wordt uitgevoerd (zoek naar **.log* bestanden). U kunt een bestand met de naam plaatsen *NLog.config* naast het uitvoerbare bestand voor uw eigen configuratie in plaats van de standaardwaarde. Zie [documentatie](https://github.com/NLog/NLog/wiki/Configuration-file#configuration-file-format) voor de beschrijving van de indeling.

Als er is geen configuratiebestand is opgegeven (dit is de standaardinstelling), lokale doorstuurserver zal worden gebruikt voor het gebruik van de standaardconfiguratie, die kan worden gevonden [hier](https://github.com/Microsoft/ApplicationInsights-LocalForwarder/blob/master/src/Common/NLog.config).

## <a name="next-steps"></a>Volgende stappen

* [Open telling](https://opencensus.io/)
