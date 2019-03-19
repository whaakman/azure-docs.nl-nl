---
title: Verbonden Factory-oplossing FAQ - Azure | Microsoft Docs
description: Veelgestelde vragen over de oplossingsverbetering voor verbonden Factory
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 12/12/2017
ms.author: dobett
ms.openlocfilehash: ed429d923cad2c715621990c146d4cf3a23e7bca
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57904746"
---
# <a name="frequently-asked-questions-for-connected-factory-solution-accelerator"></a>Veelgestelde vragen over verbonden Factory-oplossingsversnellers

Zie ook de algemene [Veelgestelde vragen over](iot-accelerators-faq.md) voor IoT-oplossingsversnellers.

### <a name="where-can-i-find-the-source-code-for-the-solution-accelerator"></a>Waar vind ik de broncode voor de oplossingsversnellers?

De broncode wordt opgeslagen in de volgende GitHub-opslagplaats:

* [Verbonden Factory-oplossingsversnellers](https://github.com/Azure/azure-iot-connected-factory)

### <a name="what-is-opc-ua"></a>Wat is de OPC UA?

OPC Unified architectuur (UA), uitgebracht in 2008, is een platform-onafhankelijke, servicegeoriënteerde interoperabiliteit standard. OPC UA wordt gebruikt door verschillende industriële systemen en apparaten, zoals de bedrijfstak van pc's, plc's en sensoren. OPC UA integreert de functionaliteit van de OPC-klassieke specificaties in één uitbreidbaar framework met ingebouwde beveiliging. Er is een standaard die wordt aangedreven door de OPC Foundation. De [OPC Foundation](https://opcfoundation.org/) is een niet-voor-profitorganisatie die meer dan 440 leden. Het doel van de organisatie, is met OPC-specificaties om interoperabiliteit van meerdere leveranciers, meerdere platforms, veilige en betrouwbare via mogelijk te maken:

* Infrastructuur
* Specificaties
* Technologie
* Processen

### <a name="why-did-microsoft-choose-opc-ua-for-the-connected-factory-solution-accelerator"></a>Waarom Microsoft OPC UA kiezen voor de oplossingsverbetering voor verbonden Factory?

Microsoft heeft ervoor gekozen OPC UA omdat het een open platform met niet-geheime, onafhankelijke, branche herkend en bewezen standaard. Het is een vereiste voor Industrie 4.0 (RAMI4.0) reference architecture oplossingen ervoor te zorgen dat de interoperabiliteit tussen een breed scala aan productieprocessen en apparatuur. Microsoft ziet vraag van klanten om Industrie 4.0-oplossingen te bouwen. Ondersteuning voor OPC UA verlaagt de drempel voor klanten om hun doelstellingen te realiseren en biedt direct bedrijfswaarde toe.

### <a name="how-do-i-add-a-public-ip-address-to-the-simulation-vm"></a>Hoe voeg ik een openbaar IP-adres aan de simulatie VM?

U hebt twee opties voor het toevoegen van het IP-adres:

* Gebruik de PowerShell-script `Simulation/Factory/Add-SimulationPublicIp.ps1` in de [opslagplaats](https://github.com/Azure/azure-iot-connected-factory). Als een parameter doorgeven in de implementatienaam van uw. Gebruik voor een lokale implementatie `<your username>ConnFactoryLocal`. Het script af te drukken om het IP-adres van de virtuele machine.

* Zoek in de Azure-portal, de resourcegroep van uw implementatie. Met uitzondering van een lokale implementatie is de resourcegroep de naam die u hebt opgegeven als oplossing of implementatie. Voor een lokale implementatie met behulp van de build-script, is de naam van de resourcegroep `<your username>ConnFactoryLocal`. Voeg nu een nieuwe **openbaar IP-adres** resource naar de resourcegroep.

> [!NOTE]
> In beide gevallen, zorg ervoor dat u de meest recente patches installeren door de instructies te volgen op de [Ubuntu website](https://wiki.ubuntu.com/Security/Upgrades). De installatie up-to-date te houden voor, zolang uw virtuele machine toegankelijk via een openbaar IP-adres is.

### <a name="how-do-i-remove-the-public-ip-address-to-the-simulation-vm"></a>Hoe kan ik het openbare IP-adres aan de simulatie VM verwijderen?

U hebt twee opties voor het verwijderen van het IP-adres:

* Gebruik het PowerShell-script Simulation/Factory/Remove-SimulationPublicIp.ps1 van de [opslagplaats](https://github.com/Azure/azure-iot-connected-factory). Als een parameter doorgeven in de implementatienaam van uw. Gebruik voor een lokale implementatie `<your username>ConnFactoryLocal`. Het script af te drukken om het IP-adres van de virtuele machine.

* Zoek in de Azure-portal, de resourcegroep van uw implementatie. Met uitzondering van een lokale implementatie is de resourcegroep de naam die u hebt opgegeven als oplossing of implementatie. Voor een lokale implementatie met behulp van de build-script, is de naam van de resourcegroep `<your username>ConnFactoryLocal`. Nu verwijderen de **openbaar IP-adres** -resource in de resourcegroep.

### <a name="how-do-i-sign-in-to-the-simulation-vm"></a>Hoe meld ik me aan de simulatie VM?

Aanmelden bij de virtuele machine van de simulatie wordt alleen ondersteund als u de oplossing met behulp van het PowerShell-script hebt geïmplementeerd `build.ps1` in de [opslagplaats](https://github.com/Azure/azure-iot-connected-factory).

Als u de oplossing van www.azureiotsolutions.com hebt geïmplementeerd, kunt u zich niet aanmelden bij de virtuele machine. U kunt zich niet in, omdat het wachtwoord wordt willekeurig gegenereerd en u deze niet herstellen.

1. Een openbaar IP-adres toevoegen aan de virtuele machine. Zie [hoe voeg ik een openbaar IP-adres toe aan de simulatie VM?](#how-do-i-remove-the-public-ip-address-to-the-simulation-vm)
1. Maak een SSH-sessie met uw virtuele machine met behulp van het IP-adres van de virtuele machine.
1. De gebruikersnaam te gebruiken is: `docker`.
1. Het wachtwoord moet worden gebruikt, is afhankelijk van de versie die u gebruikt om te implementeren:
    * Voor oplossingen die zijn geïmplementeerd met behulp van het script build.ps1 vóór 1 juni 2017, het wachtwoord is: `Passw0rd`.
    * Voor oplossingen die zijn geïmplementeerd met behulp van het script build.ps1 na 1 juni 2017, vindt u het wachtwoord in de `<name of your deployment>.config.user` bestand. Het wachtwoord wordt opgeslagen de **VmAdminPassword** instelling. Het wachtwoord willekeurig gegenereerd tijdens de implementatie, tenzij u opgeeft met behulp van de `build.ps1` parameter script `-VmAdminPassword`

### <a name="how-do-i-stop-and-start-all-docker-processes-in-the-simulation-vm"></a>Hoe ik stoppen en starten van alle docker-processen in de simulatie VM?

1. Aanmelden bij de simulatie VM. Zie [hoe meld ik me aan de simulatie VM?](#how-do-i-sign-in-to-the-simulation-vm)
1. Om te zien welke containers actief zijn, worden uitgevoerd: `docker ps`.
1. Als u wilt beëindigen alle simulatie containers, worden uitgevoerd: `./stopsimulation`.
1. Alle containers van de simulatie starten:
    * Exporteren van een shell-variabele met de naam van de **IOTHUB_CONNECTIONSTRING**. Gebruik de waarde van de **IotHubOwnerConnectionString** instellen in de `<name of your deployment>.config.user` bestand. Bijvoorbeeld:

        ```sh
        export IOTHUB_CONNECTIONSTRING="HostName={yourdeployment}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={your key}"
        ```

    * Voer `./startsimulation` uit.

### <a name="how-do-i-update-the-simulation-in-the-vm"></a>Hoe kan ik de simulatie in de virtuele machine bijwerken?

Als u wijzigingen in de simulatie aangebracht hebt, kunt u het PowerShell-script `build.ps1` in de [opslagplaats](https://github.com/Azure/azure-iot-connected-factory) met behulp van de `updatedimulation` opdracht. Met dit script alle onderdelen van de simulatie is gebaseerd, stopt de simulatie in de virtuele machine, uploadt, wordt geïnstalleerd en deze wordt gestart.

### <a name="how-do-i-find-out-the-connection-string-of-the-iot-hub-used-by-my-solution"></a>Hoe vind ik de verbindingsreeks van de IoT-hub die is gebruikt door de oplossing?

Als u uw oplossing met geïmplementeerd de `build.ps1` script in de [opslagplaats](https://github.com/Azure/azure-iot-connected-factory), de verbindingsreeks is de waarde van **IotHubOwnerConnectionString** in de `<name of your deployment>.config.user` bestand.

U vindt hier ook de connection string met behulp van de Azure portal. Zoek in de IoT Hub-resource in de resourcegroep van uw implementatie, instellingen voor connection strings.

### <a name="which-iot-hub-devices-does-the-connected-factory-simulation-use"></a>Welke IoT Hub-apparaten maakt gebruik van de verbonden Factory-simulatie?

De simulatie zelf registreert de volgende apparaten:

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

Met behulp van de [DeviceExplorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) of [de IoT-extensie voor Azure CLI](https://github.com/Azure/azure-iot-cli-extension) hulpprogramma, kunt u controleren welke apparaten zijn geregistreerd bij de IoT-hub die van uw oplossing gebruikmaakt. Apparaat als explorer wilt gebruiken, moet u de verbindingsreeks voor de IoT-hub in uw implementatie. Voor het gebruik van de IoT-extensie voor Azure CLI, moet u de naam van uw IoT-Hub.

### <a name="how-can-i-get-log-data-from-the-simulation-components"></a>Hoe vind ik logboekgegevens uit de onderdelen van de simulatie?

Alle onderdelen in de simulatie wordt informatie naar de logboekbestanden moet vastleggen. Deze bestanden kunnen worden gevonden in de virtuele machine in de map `home/docker/Logs`. Als u wilt de logboeken kunt ophalen, kunt u het PowerShell-script `Simulation/Factory/Get-SimulationLogs.ps1` in de [opslagplaats](https://github.com/Azure/azure-iot-connected-factory).

Met dit script moet zich aanmelden bij de virtuele machine. Mogelijk moet u referenties opgeven voor de aanmelding. Zie [hoe meld ik me aan de virtuele machine van de simulatie?](#how-do-i-sign-in-to-the-simulation-vm) te vinden van de referenties.

Het script voegt/Hiermee verwijdert u een openbaar IP-adres aan de virtuele machine, als deze nog geen een en wordt deze verwijderd. Het script worden alle logboekbestanden geplaatst in een archief en het archief downloadt naar uw ontwikkelwerkstation.

U kunt ook aanmelden bij de virtuele machine via SSH en controleren van de logboekbestanden tijdens runtime.

### <a name="how-can-i-check-if-the-simulation-is-sending-data-to-the-cloud"></a>Hoe kan ik controleren als de simulatie gegevens naar de cloud verzendt?

Met de [DeviceExplorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) of de [-gebeurtenissen van Azure IoT CLI-extensie controleren](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-hub-monitor-events) opdracht, kunt u de gegevens die worden verzonden naar IoT Hub van bepaalde apparaten controleren. Voor het gebruik van deze hulpprogramma's, moet u weten de verbindingsreeks voor de IoT-hub in uw implementatie. Zie [hoe vind ik uit de verbindingsreeks van de IoT-hub die is gebruikt door de oplossing?](#how-do-i-find-out-the-connection-string-of-the-iot-hub-used-by-my-solution)

Controleer de gegevens die worden verzonden door een van de uitgever-apparaten:

* publisher.beijing.corp.contoso
* publisher.capetown.corp.contoso
* publisher.mumbai.corp.contoso
* publisher.munich0.corp.contoso
* publisher.rio.corp.contoso
* publisher.seattle.corp.contoso

Als er geen gegevens worden verzonden naar IoT Hub, is er een probleem met de simulatie. Als een eerste stap voor analyse, moet u de logboekbestanden van de onderdelen van de simulatie analyseren. Zie [hoe vind ik logboekgegevens uit de onderdelen van de simulatie?](#how-can-i-get-log-data-from-the-simulation-components) Probeer vervolgens om te stoppen en starten van de simulatie en als er nog steeds geen gegevens verzonden zijn, werkt u de simulatie volledig. Zie [hoe kan ik de simulatie in de virtuele machine bijwerken?](#how-do-i-update-the-simulation-in-the-vm)

### <a name="how-do-i-enable-an-interactive-map-in-my-connected-factory-solution"></a>Hoe kan ik een interactieve kaart inschakelen in Mijn oplossing voor verbonden Factory?

Als u wilt een interactieve kaart in uw oplossing voor verbonden Factory's inschakelen, moet u een Azure kaarten-account hebben.

Bij het implementeren van [www.azureiotsolutions.com](https://www.azureiotsolutions.com), het implementatieproces wordt een Azure kaarten-account toegevoegd aan de resourcegroep met de solution accelerator-services.

Wanneer u implementeert met behulp van de `build.ps1` script in de verbonden Factory GitHub-opslagplaats van de omgevingsvariabele `$env:MapApiQueryKey` in het venster bouwen aan de [sleutel van uw Azure kaarten-account](../azure-maps/how-to-manage-account-keys.md). De interactieve kaart wordt automatisch ingeschakeld.

U kunt ook een accountsleutel voor Azure-kaarten toevoegen aan uw oplossingenaccelerator na de implementatie. Navigeer naar de Azure-portal en toegang tot de resource App Service in uw verbonden Factory-implementatie. Navigeer naar **toepassingsinstellingen**, waar u een sectie vinden **toepassingsinstellingen**. Stel de **MapApiQueryKey** naar de [sleutel van uw Azure kaarten-account](../azure-maps/how-to-manage-account-keys.md). Sla de instellingen op en navigeer vervolgens naar **overzicht** en de App-Service opnieuw.

### <a name="how-do-i-create-an-azure-maps-account"></a>Hoe maak ik een Azure kaarten-account?

Zie, [over het beheren van uw Azure kaarten-account en sleutels](../azure-maps/how-to-manage-account-keys.md).

### <a name="how-to-obtain-your-azure-maps-account-key"></a>Het ophalen van de sleutel van uw Azure-kaarten-account

Zie, [over het beheren van uw Azure kaarten-account en sleutels](../azure-maps/how-to-manage-account-keys.md).

### <a name="how-do-enable-the-interactive-map-while-debugging-locally"></a>Hoe schakel de interactieve kaart lokaal opsporen van fouten?

Als u wilt de interactieve kaart inschakelen tijdens de foutopsporing lokaal, stel de waarde van de instelling `MapApiQueryKey` in de bestanden `local.user.config` en `<yourdeploymentname>.user.config` in de hoofdmap van uw implementatie met de waarde van de **querysleutel** u gekopieerd eerder.

### <a name="how-do-i-use-a-different-image-at-the-home-page-of-my-dashboard"></a>Hoe gebruik ik een andere installatiekopie op de startpagina van Mijn dashboard?

De statische afbeelding weergegeven i/o wijzigen de startpagina van het dashboard, de installatiekopie vervangen `WebApp\Content\img\world.jpg`. Vervolgens opnieuw maken en implementeren van de Web-App.

### <a name="how-do-i-use-non-opc-ua-devices-with-connected-factory"></a>Hoe gebruik ik niet OPC UA-apparaten met verbonden Factory?

Voor het verzenden van telemetriegegevens van niet OPC UA apparaten naar verbonden Factory:

1. [Een nieuw station configureren in de verbonden Factory-topologie](iot-accelerators-connected-factory-configure.md) in de `ContosoTopologyDescription.json` bestand.

1. Opname van de telemetriegegevens die zijn verbonden Factory compatibel JSON-indeling:

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

1. De indeling van `<timestamp>` is: `2017-12-08T19:24:51.886753Z`

1. Start de Service van de App Connected Factory.

### <a name="next-steps"></a>Volgende stappen

U kunt ook enkele van de andere functies en mogelijkheden van de IoT-oplossingsversnellers bekijken:

* [Een overzicht van de oplossingsversneller voor Predictief onderhoud](iot-accelerators-predictive-overview.md)
* [Verbonden Factory oplossingsversnellers implementeren](quickstart-connected-factory-deploy.md)
* [Fundamentele IoT-beveiliging](/azure/iot-fundamentals/iot-security-ground-up)
