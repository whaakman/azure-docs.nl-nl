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
ms.date: 12/12/2017
ms.author: dobett
ms.openlocfilehash: ab72152fc937e3c4552147fce29c95ea0efcadf4
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
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

Microsoft heeft ervoor gekozen OPC UA omdat het een open, op niet-bedrijfseigen platform onafhankelijke bedrijfstak herkend en beproefde standaard. Het is een vereiste voor Industrie 4.0 (RAMI4.0) reference architecture oplossingen interoperabiliteit tussen een uitgebreide reeks productieprocessen en apparatuur. Microsoft ziet vraag van klanten Industrie 4.0 oplossingen bouwen. Ondersteuning voor OPC UA verlaagt u de blokkade voor klanten om hun doelstellingen te realiseren en biedt onmiddellijke bedrijfswaarde toe.

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

* proxy.beijing.corp.contoso
* proxy.capetown.corp.contoso
* proxy.mumbai.corp.contoso
* proxy.munich0.corp.contoso
* proxy.rio.corp.contoso
* proxy.seattle.corp.contoso
* publisher.beijing.corp.contoso
* publisher.capetown.corp.contoso
* publisher.mumbai.corp.contoso
* publisher.munich0.corp.contoso
* publisher.rio.corp.contoso
* publisher.seattle.corp.contoso

Met behulp van de [DeviceExplorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) of [de IoT-extensie voor Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension) hulpprogramma, u kunt controleren welke apparaten zijn geregistreerd bij de IoT-hub die van uw oplossing gebruikmaakt. Om het apparaat explorer gebruikt, moet u de verbindingsreeks voor de IoT-hub in uw implementatie. Voor het gebruik van de IoT-extensie voor Azure CLI 2.0, moet u de naam van uw IoT-Hub.

### <a name="how-can-i-get-log-data-from-the-simulation-components"></a>Hoe krijg ik logboekgegevens uit de simulatie-onderdelen

Alle onderdelen in de simulatie aanmelden informatie naar de logboekbestanden. Deze bestanden vindt u in de virtuele machine in de map `home/docker/Logs`. Voor het ophalen van de logboeken, kunt u het PowerShell-script `Simulation/Factory/Get-SimulationLogs.ps1` in de [opslagplaats](https://github.com/Azure/azure-iot-connected-factory).

Dit script moet zich aanmelden bij de virtuele machine. Mogelijk moet u referenties opgeven voor de aanmeldingspagina. Zie [hoe meld ik me bij de simulatie VM?](#how-do-i-sign-in-to-the-simulation-vm) vinden van de referenties.

Het script toevoegen of eruit verwijderen een openbaar IP-adres aan de virtuele machine, als deze nog niet over een en wordt deze verwijderd. Het script worden alle logboekbestanden geplaatst in een archief en downloadt het archief naar uw ontwikkelwerkstation.

U kunt ook aanmelden bij de virtuele machine via SSH en controleren van de logboekbestanden tijdens runtime.

### <a name="how-can-i-check-if-the-simulation-is-sending-data-to-the-cloud"></a>Hoe kan ik controleren als de simulatie gegevens naar de cloud verzendt?

Met de [DeviceExplorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) of de [iothub explorer](https://github.com/azure/iothub-explorer) hulpprogramma, kunt u de gegevens die worden verzonden naar IoT Hub van bepaalde apparaten controleren. Voor het gebruik van deze hulpprogramma's die u wilt weten van de verbindingsreeks voor de IoT-hub in uw implementatie. Zie [hoe vind ik uit de verbindingsreeks van de IoT-hub die wordt gebruikt door Mijn-oplossing?](#how-do-i-find-out-the-connection-string-of-the-iot-hub-used-by-my-solution)

Inspecteer de gegevens die worden verzonden door een van de uitgever van apparaten:

* publisher.beijing.corp.contoso
* publisher.capetown.corp.contoso
* publisher.mumbai.corp.contoso
* publisher.munich0.corp.contoso
* publisher.rio.corp.contoso
* publisher.seattle.corp.contoso

Als er geen gegevens die worden verzonden naar IoT Hub, is er een probleem met de simulatie. Als eerste analyse stap moet u de logboekbestanden van de onderdelen van de simulatie analyseren. Zie [hoe krijg ik logboekgegevens uit de onderdelen van de simulatie?](#how-can-i-get-log-data-from-the-simulation-components) Probeer vervolgens om te stoppen en starten van de simulatie en als er nog geen gegevens verzonden zijn, werkt de simulatie volledig. Zie [hoe kan ik de simulatie in de virtuele machine bijwerken?](#how-do-i-update-the-simulation-in-the-vm)

### <a name="how-do-i-enable-an-interactive-map-in-my-connected-factory-solution"></a>Hoe schakel ik een interactieve hyperlinks in mijn verbonden factory-oplossing?

Om een interactieve kaart in uw verbonden factory-oplossing, moet u een bestaande Bing kaarten-API voor Enterprise-abonnement hebben.

Bij het implementeren van [www.azureiotsuite.com](http://www.azureiotsuite.com), het implementatieproces wordt gecontroleerd of uw abonnement een ingeschakelde Bing kaarten-API voor Enterprise-plan heeft en implementeert automatisch een interactieve hyperlinks in verbonden factory. Als dit niet het geval is, kunt u nog steeds inschakelen een interactieve hyperlinks in uw implementatie als volgt:

Wanneer u implementeert met behulp van de `build.ps1` script in de verbonden fabriek GitHub-opslagplaats en u een Bing kaarten-API voor Enterprise-abonnement hebt, stelt u de omgevingsvariabele `$env:MapApiQueryKey` in het venster build voor de querysleutel van uw abonnement. De interactieve kaart wordt automatisch ingeschakeld.

Als u geen Bing kaarten-API voor Enterprise-abonnement hebt, implementeert u de verbonden factory-oplossing van [www.azureiotsuite.com](http://www.azureiotsuite.com) of met behulp van de `build.ps1` script. Bing kaarten-API voor Enterprise-plan vervolgens toevoegen aan uw abonnement zoals toegelicht in [hoe maak ik een Bing kaarten-API voor Enterprise-account?](#how-do-i-create-a-bing-maps-api-for-enterprise-account). Opzoeken van de querysleutel van dit account, zoals wordt beschreven in [het verkrijgen van uw Bing kaarten-API voor Enterprise-querysleutel](#how-to-obtain-your-bing-maps-api-for-enterprise-querykey) en deze sleutel op te slaan. Navigeer naar de Azure-portal en toegang tot de App Service-bron in uw implementatie verbonden factory. Navigeer naar **toepassingsinstellingen**, waar u een sectie vinden **appinstellingen**. Stel de **MapApiQueryKey** aan de querysleutel die u hebt verkregen. Sla de instellingen en navigeer vervolgens naar **overzicht** en start de App-Service opnieuw.

### <a name="how-do-i-create-a-bing-maps-api-for-enterprise-account"></a>Hoe maak ik een Bing kaarten-API voor Enterprise-account

U krijgt een gratis *interne transacties niveau 1 Bing-kaarten voor Enterprise* plan. U kunt echter alleen toevoegen twee van deze plannen op een Azure-abonnement. Als u geen Bing kaarten-API voor Enterprise-account hebt, maakt u in de Azure-portal door te klikken op **+ maken van een resource**. Zoek vervolgens naar **Bing kaarten-API voor Enterprise** en volg de aanwijzingen om deze te maken.

![Bing-sleutel](media/iot-suite-faq-cf/bing.png)

### <a name="how-to-obtain-your-bing-maps-api-for-enterprise-querykey"></a>Het verkrijgen van uw Bing kaarten-API voor Enterprise-querysleutel

Nadat u uw Bing kaarten-API voor Enterprise-abonnement hebt gemaakt, moet u een Bing-kaarten voor Enterprise resource toevoegen aan de resourcegroep van uw oplossing verbonden factory in de Azure-portal.

1. Ga in de Azure-portal naar de resourcegroep die uw Bing kaarten-API voor Enterprise-plan bevat.

1. Klik op **alle instellingen**, klikt u vervolgens **Sleutelbeheer**.

1. Er zijn twee sleutels: **hoofdsleutel** en **querysleutel**. Kopieer de **querysleutel** waarde.

1. De sleutel die is opgenomen door de `build.ps1` script, stelt u de omgevingsvariabele `$env:MapApiQueryKey` in uw PowerShell-omgeving naar de **querysleutel** van uw abonnement. Het build-script vervolgens automatisch wordt de waarde toegevoegd aan de instellingen van de App Service.

1. Uitvoeren van een lokale of cloud implementatie met de `build.ps1` script.

### <a name="how-do-enable-the-interactive-map-while-debugging-locally"></a>Hoe schakel de interactieve kaart tijdens foutopsporing lokaal?

Om de interactieve kaart terwijl u lokaal fouten opspoort, stel de waarde van de instelling `MapApiQueryKey` in de bestanden `local.user.config` en `<yourdeploymentname>.user.config` in de hoofdmap van uw implementatie om de waarde van de **querysleutel** u gekopieerd eerder.

### <a name="how-do-i-use-a-different-image-at-the-home-page-of-my-dashboard"></a>Hoe gebruik ik een andere installatiekopie op de startpagina van Mijn dashboard

De statische afbeelding weergegeven io wijzigen de startpagina van het dashboard, vervang de installatiekopie `WebApp\Content\img\world.jpg`. Vervolgens opnieuw maken en implementeren van de Web-App.

### <a name="how-do-i-use-non-opc-ua-devices-with-connected-factory"></a>Hoe gebruik ik niet OPC UA-apparaten met verbonden factory

Telemetriegegevens te verzenden vanaf niet OPC UA apparaten tot verbonden factory:

1. [Een nieuw station configureren in de topologie verbonden factory](iot-suite-connected-factory-configure.md) in de `ContosoTopologyDescription.json` bestand.

1. De telemetriegegevens verbonden factory compatibel JSON-indeling voor opnemen:

    ```json
    [
      {
        "ApplicationUri": "<the_value_of_OpcUri_of_your_station",
        "DisplayName": "<name_of_the_datapoint>",
        "NodeId": "value_of_NodeId_of_your_datapoint_in_the_station",
        "Value": {
          "Value": <datapoint_value>,
          "SourceTimestamp": "<timestamp>"
        }
      }
    ]
    ```

1. De indeling van `<timestamp>` is:`2017-12-08T19:24:51.886753Z`

1. Start opnieuw op de verbonden App Service-factory.

### <a name="next-steps"></a>Volgende stappen

U kunt ook enkele van de andere functies en mogelijkheden van de vooraf geconfigureerde IoT Suite-oplossingen verkennen:

* [Overzicht van voorspeld onderhoud vooraf geconfigureerde oplossing](iot-suite-predictive-overview.md)
* [Overzicht van de verbonden factory vooraf geconfigureerde oplossing](iot-suite-connected-factory-overview.md)
* [Beveiliging van een compleet nieuwe IoT](securing-iot-ground-up.md)