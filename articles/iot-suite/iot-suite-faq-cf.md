---
title: Factory-oplossing FAQ - Azure verbonden | Microsoft Docs
description: Veelgestelde vragen voor verbonden IoT Suite-factory
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/10/2017
ms.author: corywink
ms.openlocfilehash: d4cb452b34ddefc70dc1adcff0e5fead072aa16a
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2017
---
# <a name="frequently-asked-questions-for-iot-suite-connected-factory-preconfigured-solution"></a>Veelgestelde vragen voor verbonden factory IoT Suite vooraf geconfigureerde oplossing

Zie ook de algemene [Veelgestelde vragen over](iot-suite-faq.md) voor IoT Suite.

### <a name="where-can-i-find-the-source-code-for-the-preconfigured-solution"></a>Waar vind ik de broncode voor de vooraf geconfigureerde oplossing

De broncode is opgeslagen in de volgende GitHub-opslagplaats:

* [Verbonden factory vooraf geconfigureerde oplossing](https://github.com/Azure/azure-iot-connected-factory)

### <a name="what-is-opc-ua"></a>Wat is OPC UA?

OPC Unified architectuur (UA), uitgebracht in 2008, is een standaard platformonafhankelijk, servicegerichte interoperabiliteit. OPC UA wordt gebruikt door verschillende industriële systemen en apparaten zoals bedrijfstak pc's, plc's en sensoren. OPC UA integreert de functionaliteit van de klassieke OPC specificaties in één uitbreidbaar framework met ingebouwde beveiliging. Het is een standaard die is aangedreven door het OPC-Foundation. De [OPC Foundation](http://opcfoundation.org/) is een organisatie zonder met meer dan 440 leden. Het doel van de organisatie is het OPC-specificaties gebruiken voor meerdere leveranciers, meerdere platforms, veilige en betrouwbare interoperabiliteit via:

* Infrastructuur
* Specificaties
* Technologie
* Processen

### <a name="why-did-microsoft-choose-opc-ua-for-the-connected-factory-preconfigured-solution"></a>Waarom Microsoft OPC UA kiezen voor de verbonden factory vooraf geconfigureerde oplossing?

Microsoft heeft ervoor gekozen OPC UA omdat het een open, op niet-bedrijfseigen platform onafhankelijke bedrijfstak herkend en beproefde standaard. Het is een vereiste voor Industrie 4.0 (RAMI4.0) reference architecture oplossingen interoperabiliteit tussen een uitgebreide reeks productieprocessen en apparatuur. Microsoft ziet vraag van onze klanten om Industrie 4.0 oplossingen te maken. Ondersteuning voor OPC UA verlaagt u de blokkade voor klanten om hun doelstellingen te realiseren en biedt onmiddellijke bedrijfswaarde toe.

### <a name="how-do-i-add-a-public-ip-address-to-the-simulation-vm"></a>Hoe voeg ik een openbaar IP-adres toe aan de simulatie VM?

U hebt twee opties voor het IP-adres toevoegen:

* Gebruik de PowerShell-script `Simulation/Factory/Add-SimulationPublicIp.ps1` in de [opslagplaats](https://github.com/Azure/azure-iot-connected-factory). Als een parameter doorgeven in de implementatienaam van uw. Gebruik voor een lokale implementatie `<your username>ConnFactoryLocal`. Het script wordt het IP-adres van de virtuele machine afgedrukt.

* Zoek de resourcegroep van uw implementatie in de Azure-portal. Met uitzondering van een lokale implementatie is de resourcegroep de naam die u hebt opgegeven als oplossing of implementatie. Voor een lokale implementatie met behulp van de build-script, de naam van de resourcegroep is `<your username>ConnFactoryLocal`. Voeg nu een nieuwe **openbaar IP-adres** resource toe aan de resourcegroep.

> [!NOTE]
> In beide gevallen Zorg ervoor dat u de meest recente patches installeren door de instructies op de [Ubuntu website](https://wiki.ubuntu.com/Security/Upgrades). De installatie up-to-date te houden voor zolang uw VM toegankelijk via een openbaar IP-adres is.

### <a name="how-do-i-remove-the-public-ip-address-to-the-simulation-vm"></a>Hoe kan ik het openbare IP-adres voor de simulatie VM verwijderen?

U hebt twee opties voor het verwijderen van het IP-adres:

* Gebruik het PowerShell-script Simulation/Factory/Remove-SimulationPublicIp.ps1 van de [opslagplaats](https://github.com/Azure/azure-iot-connected-factory). Als een parameter doorgeven in de implementatienaam van uw. Gebruik voor een lokale implementatie `<your username>ConnFactoryLocal`. Het script wordt het IP-adres van de virtuele machine afgedrukt.

* Zoek de resourcegroep van uw implementatie in de Azure-portal. Met uitzondering van een lokale implementatie is de resourcegroep de naam die u hebt opgegeven als oplossing of implementatie. Voor een lokale implementatie met behulp van de build-script, de naam van de resourcegroep is `<your username>ConnFactoryLocal`. Verwijder nu het **openbaar IP-adres** resource uit de resourcegroep.

### <a name="how-do-i-sign-in-to-the-simulation-vm"></a>Hoe meld ik me aan de simulatie VM?

Aanmelden bij de simulatie VM wordt alleen ondersteund als u uw oplossing met behulp van het PowerShell-script hebt geïmplementeerd `build.ps1` in de [opslagplaats](https://github.com/Azure/azure-iot-connected-factory).

Als u de oplossing van www.azureiotsuite.com hebt geïmplementeerd, kunt u zich niet aanmelden met de virtuele machine. Je aanmelden niet, omdat het wachtwoord wordt willekeurig gegenereerd en kunt u deze niet herstellen.

1. Een openbaar IP-adres toevoegen aan de virtuele machine. Zie [hoe voeg ik een openbaar IP-adres toe aan de simulatie VM?](#how-do-i-remove-the-public-ip-address-to-the-simulation-vm)
1. Maak een SSH-sessie met uw virtuele machine met behulp van het IP-adres van de virtuele machine.
1. De gebruikersnaam te gebruiken: `docker`.
1. Het wachtwoord te gebruiken, is afhankelijk van de versie die u gebruikt om te implementeren:
    * Voor oplossingen die zijn geïmplementeerd met behulp van het script build.ps1 vóór 1 juni 2017, het wachtwoord is: `Passw0rd`.
    * Voor oplossingen die zijn geïmplementeerd met behulp van het script build.ps1 na 1 juni 2017, vindt u het wachtwoord in de `<name of your deployment>.config.user` bestand. Het wachtwoord wordt opgeslagen in de **VmAdminPassword** instelling. Het wachtwoord willekeurig gegenereerd tijdens de implementatie tenzij u opgeeft met behulp van de `build.ps1` parameter script`-VmAdminPassword`

### <a name="how-do-i-stop-and-start-all-docker-processes-in-the-simulation-vm"></a>Hoe ik stoppen en starten van alle docker-processen in de simulatie VM?

1. Aanmelden bij de simulatie VM. Zie [hoe meld ik me bij de simulatie VM?](#how-do-i-sign-in-to-the-simulation-vm)
1. Als u wilt controleren welke containers actief zijn, worden uitgevoerd: `docker ps`.
1. Als u wilt stoppen alle simulatie containers, uitvoeren: `./stopsimulation`.
1. Alle containers van de simulatie starten:
    * Exporteren van een shell-variabele met de naam **IOTHUB_CONNECTIONSTRING**. Gebruik de waarde van de **IotHubOwnerConnectionString** instellen in de `<name of your deployment>.config.user` bestand. Bijvoorbeeld:

        ```
        export IOTHUB_CONNECTIONSTRING="HostName={yourdeployment}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={your key}"
        ```

    * Voer `./startsimulation` uit.

### <a name="how-do-i-update-the-simulation-in-the-vm"></a>Hoe kan ik de simulatie in de virtuele machine bijwerken?

Als u wijzigingen in de simulatie aangebracht hebt, kunt u het PowerShell-script `build.ps1` in de [opslagplaats](https://github.com/Azure/azure-iot-connected-factory) met behulp van de `updatedimulation` opdracht. Dit script builds van alle onderdelen van de simulatie, stopt de simulatie in de virtuele machine, uploadt, installeert en deze wordt gestart.

### <a name="how-do-i-find-out-the-connection-string-of-the-iot-hub-used-by-my-solution"></a>Hoe vind ik de verbindingsreeks van de IoT-hub die wordt gebruikt door Mijn-oplossing?

Als u uw oplossing met geïmplementeerd de `build.ps1` script in de [opslagplaats](https://github.com/Azure/azure-iot-connected-factory), de verbindingsreeks is de waarde van **IotHubOwnerConnectionString** in de `<name of your deployment>.config.user` bestand.

Ook vindt u de verbindingsreeks met de Azure portal. Zoek in de resource IoT-Hub in de resourcegroep van uw implementatie, instellingen voor de verbindingstekenreeks.

### <a name="which-iot-hub-devices-does-the-connected-factory-simulation-use"></a>Welke apparaten IoT-Hub maakt gebruik van de simulatie verbonden factory?

De simulatie zichzelf registreert de volgende apparaten:

* proxy.Beijing.corp.contoso
* proxy.capetown.corp.contoso
* proxy.Mumbai.corp.contoso
* proxy.munich0.corp.contoso
* proxy.Rio.corp.contoso
* proxy.Seattle.corp.contoso
* Publisher.Beijing.corp.contoso
* Publisher.capetown.corp.contoso
* Publisher.Mumbai.corp.contoso
* Publisher.munich0.corp.contoso
* Publisher.Rio.corp.contoso
* Publisher.Seattle.corp.contoso

Met behulp van de [DeviceExplorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) of [iothub explorer](https://github.com/azure/iothub-explorer) hulpprogramma, u kunt controleren welke apparaten zijn geregistreerd bij de IoT-hub die van uw oplossing gebruikmaakt. Voor het gebruik van deze hulpprogramma's, moet u de verbindingsreeks voor de IoT-hub in uw implementatie.

### <a name="how-can-i-get-log-data-from-the-simulation-components"></a>Hoe krijg ik logboekgegevens uit de simulatie-onderdelen

Alle onderdelen in de simulatie aanmelden informatie naar de logboekbestanden. Deze bestanden vindt u in de virtuele machine in de map `home/docker/Logs`. Voor het ophalen van de logboeken, kunt u het PowerShell-script `Simulation/Factory/Get-SimulationLogs.ps1` in de [opslagplaats](https://github.com/Azure/azure-iot-connected-factory).

Dit script moet zich aanmelden bij de virtuele machine. Mogelijk moet u referenties opgeven voor de aanmeldingspagina. Zie [hoe meld ik me bij de simulatie VM?](#how-do-i-sign-in-to-the-simulation-vm) vinden van de referenties.

Het script toevoegen of eruit verwijderen een openbaar IP-adres aan de virtuele machine, als deze nog niet over een en wordt deze verwijderd. Het script worden alle logboekbestanden geplaatst in een archief en downloadt het archief naar uw ontwikkelwerkstation.

U kunt ook aanmelden bij de virtuele machine via SSH en controleren van de logboekbestanden tijdens runtime.

### <a name="how-can-i-check-if-the-simulation-is-sending-data-to-the-cloud"></a>Hoe kan ik controleren als de simulatie gegevens naar de cloud verzendt?

Met de [DeviceExplorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) of de [iothub explorer](https://github.com/azure/iothub-explorer) hulpprogramma, kunt u de gegevens die worden verzonden naar IoT Hub van bepaalde apparaten controleren. Voor het gebruik van deze hulpprogramma's die u wilt weten van de verbindingsreeks voor de IoT-hub in uw implementatie. Zie [hoe vind ik uit de verbindingsreeks van de IoT-hub die wordt gebruikt door Mijn-oplossing?](#how-do-i-find-out-the-connection-string-of-the-iot-hub-used-by-my-solution)

Inspecteer de gegevens die worden verzonden door een van de uitgever van apparaten:

* Publisher.Beijing.corp.contoso
* Publisher.capetown.corp.contoso
* Publisher.Mumbai.corp.contoso
* Publisher.munich0.corp.contoso
* Publisher.Rio.corp.contoso
* Publisher.Seattle.corp.contoso

Als er geen gegevens die worden verzonden naar IoT Hub, is er een probleem met de simulatie. Als eerste analyse stap moet u de logboekbestanden van de onderdelen van de simulatie analyseren. Zie [hoe krijg ik logboekgegevens uit de onderdelen van de simulatie?](#how-can-i-get-log-data-from-the-simulation-components) Probeer vervolgens om te stoppen en starten van de simulatie en als er nog geen gegevens verzonden zijn, werkt de simulatie volledig. Zie [hoe kan ik de simulatie in de virtuele machine bijwerken?](#how-do-i-update-the-simulation-in-the-vm)

### <a name="next-steps"></a>Volgende stappen

U kunt ook enkele van de andere functies en mogelijkheden van de vooraf geconfigureerde IoT Suite-oplossingen verkennen:

* [Overzicht van voorspeld onderhoud vooraf geconfigureerde oplossing](iot-suite-predictive-overview.md)
* [Overzicht van de verbonden factory vooraf geconfigureerde oplossing](iot-suite-connected-factory-overview.md)
* [Beveiliging van een compleet nieuwe IoT](securing-iot-ground-up.md)