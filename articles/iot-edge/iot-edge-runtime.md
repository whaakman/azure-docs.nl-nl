---
title: Inzicht in de Azure IoT rand runtime | Microsoft Docs
description: Meer informatie over de Azure IoT Edge-runtime en hoe deze uw randapparaten machtigt
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 10/05/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 4727560df897f6c1a0aaa6d7f5d4e1c76fc02a46
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2017
---
# <a name="understand-the-azure-iot-edge-runtime-and-its-architecture---preview"></a>Inzicht in de Azure IoT Edge-runtime en de bijbehorende architectuur - voorbeeld

De rand van de IoT-runtime is een verzameling van programma's die moeten worden geïnstalleerd op een apparaat om te worden beschouwd als een Edge van de IoT-apparaat. Gezamenlijk de onderdelen van de rand van de IoT-runtime rand van de IoT-apparaten ontvangen code om uit te voeren aan de rand inschakelen en kennis van de resultaten. 

De runtime IoT rand voert de volgende functies op de IoT Edge-apparaten:

* Installeert workloads op het apparaat en werkt deze bij.
* Onderhoudt de Azure IoT Edge-beveiligingsstandaarden op het apparaat.
* Zorgt ervoor dat [IoT rand modules][lnk-modules] altijd worden uitgevoerd.
* Rapporteert de status van de module aan de cloud voor externe bewaking.
* Vergemakkelijkt de communicatie tussen downstream leaf-apparaten en het IoT Edge-apparaat.
* Vergemakkelijkt de communicatie tussen modules op het IoT Edge-apparaat.
* Vergemakkelijkt de communicatie tussen het IoT Edge-apparaat en de cloud.

![IoT-rand runtime communiceert insights en de status van de module naar IoT Hub][1]

De verantwoordelijkheden van de rand van de IoT-runtime is onderverdeeld in twee categorieën: module Beheer en communicatie. Deze twee rollen worden uitgevoerd door twee onderdelen die gezamenlijk de rand van de IoT-runtime. De rand van de IoT-hub is verantwoordelijk voor communicatie gebruikt, terwijl de rand van de IoT-agent implementeren en bewaken van de modules beheert. 

Zowel de Edge-agent en de Edge-hub zijn modules, net als elke andere module uitgevoerd op een Edge van de IoT-apparaat. Zie voor meer informatie over de werking van modules [lnk-modules]. 

## <a name="iot-edge-hub"></a>Rand van de IoT hub

De Edge-hub is een van twee modules die gezamenlijk de rand van Azure IoT-runtime. Het fungeert als een lokale proxyserver voor IoT Hub bij het blootstellen van de dezelfde protocoleindpunten als IoT Hub. Deze consistentie betekent dat clients (of apparaten of modules) kunnen verbinding maken met de rand van de IoT-runtime, net als met IoT Hub. 

>[!NOTE]
> Rand Hub ondersteunt alleen clients die verbinding maken met behulp van protocollen MQTT tijdens de openbare preview.

De Edge-hub is niet een volledige versie van de IoT-Hub die lokaal wordt uitgevoerd. Er zijn een aantal zaken die de hub rand achtergrond naar IoT Hub delegeert. Rand hub verzendt bijvoorbeeld verificatieaanvragen naar IoT Hub wanneer een apparaat de eerste keer probeert verbinding maken. Nadat de eerste verbinding tot stand is gebracht, informatie over beveiliging in de cache geplaatst lokaal door rand hub. Volgende verbindingen vanaf dat apparaat zijn zonder te verifiëren naar de cloud toegestaan. 

>[!NOTE]
> Tijdens de openbare preview van worden de runtime verbonden telkens wanneer wordt geprobeerd om een apparaat te verifiëren.

Als u wilt de bandbreedte beperken de rand van de IoT-oplossing gebruikt, de Edge-hub optimaliseert het aantal werkelijke verbindingen worden aangebracht in de cloud. Rand hub logische verbindingen van clients, zoals modules of leaf-apparaten neemt en worden deze gecombineerd voor één fysieke verbinding met de cloud. De details van dit proces zijn transparant voor de rest van de oplossing. Clients zien dat ze hun eigen verbinding naar de cloud hebben, zelfs als ze al worden verzonden via dezelfde verbinding. 

![Rand hub fungeert als een gateway tussen meerdere fysieke apparaten en de cloud][2]

Edge-hub kunt bepalen of deze verbonden met IoT Hub. Als de verbinding verbroken wordt, Edge hub berichten of twin updates lokaal worden opgeslagen. Zodra een verbinding opnieuw tot stand is gebracht, wordt deze alle gegevens gesynchroniseerd. De locatie die wordt gebruikt voor deze tijdelijke cache wordt bepaald door een eigenschap van de Edge-hub module twin. De grootte van de cache is niet beperkt, en zullen groeien zolang het apparaat heeft opslagcapaciteit. 

>[!NOTE]
>Toevoegen van de controle over extra parameters in cache wordt worden toegevoegd aan het product voordat deze in de algemene beschikbaarheid.

### <a name="module-communication"></a>Module-communicatie

Rand Hub vergemakkelijkt de communicatie van de module-module. Rand Hub gebruikt als broker bericht houdt modules onafhankelijk van elkaar. Modules hoeft alleen te geven van de invoer waarop ze berichten en de uitvoer waarnaar ze berichten schrijven accepteren. Een ontwikkelaar van de oplossing vervolgens dan aan elkaar gehecht deze invoer en uitvoer samen zodat de modules gegevens in de volgorde die specifiek zijn voor deze oplossing verwerken. 

![Rand Hub vergemakkelijkt de communicatie van de module-naar-module][3]

Om gegevens te verzenden naar de rand hub, wordt in een module de SendEventAsync-methode aanroept. Het eerste argument geeft op welke uitvoer om het bericht te verzenden. De volgende pseudocode verzendt een bericht op output1:

    DeviceClient client = new DeviceClient.CreateFromConnectionString(moduleConnectionString, settings); 
    await client.OpenAsync(); 
    await client.SendEventAsync(“output1”, message); 

Registreren voor het ontvangen van een bericht een callback die binnenkort in op een specifieke invoer berichten worden verwerkt. De volgende pseudocode registreert de functie messageProcessor moet worden gebruikt voor het verwerken van alle berichten ontvangen op input1:

    await client.SetEventHandlerAsync(“input1”, messageProcessor, userContext);
    
De ontwikkelaar van de oplossing is verantwoordelijk voor het opgeven van de regels die bepalen hoe de rand hub berichten tussen modules doorstuurt. Routeringsregels zijn gedefinieerd in de cloud en omlaag naar het Edge-hub in de twin apparaat gepusht. Dezelfde syntaxis voor IoT Hub routes wordt gebruikt voor het definiëren van routes tussen modules in Azure IoT rand. 

<!--- For more info on how to declare routes between modules, see []. --->   

![Routes tussen modules][4]

## <a name="iot-edge-agent"></a>Rand van de IoT-agent

De rand van de IoT-agent is de module waaruit de rand van Azure IoT-runtime. Het is verantwoordelijk voor het instantiëren van modules, ervoor te zorgen dat ze worden uitgevoerd en de rapportage van de status van de modules terug naar IoT Hub. Net als elke andere module worden de Edge-agent de twin module gebruikt voor het opslaan van deze configuratiegegevens. 

Om te beginnen met het uitvoeren van de Edge-agent, voer de azure-iot-edge-runtime-ctl.py start-opdracht. De agent opgehaald van de module-twin uit IoT Hub en inspecteert de woordenlijst modules. Het woordenboek modules is de verzameling van modules die moet worden gestart. 

Elk item in de woordenlijst modules bevat specifieke informatie over een module en door de agent rand wordt gebruikt voor het beheren van de levenscyclus van de module. Enkele van de interessanter eigenschappen zijn: 

* **Settings.Image** – de container-installatiekopie die gebruikmaakt van de Edge-agent starten van de module. De Edge-agent moet worden geconfigureerd met de referenties voor het register van de container als de installatiekopie is beveiligd met een wachtwoord. Gebruik de volgende opdracht voor het configureren van de Edge-agent:`azure-iot-edge-runtime-ctl.py –configure`
* **settings.createOptions** : een tekenreeks die rechtstreeks aan de Docker-daemon wordt doorgegeven bij het starten van een module-container. Toevoegen van Docker-opties in deze eigenschap kunt u geavanceerde opties zoals poort doorsturen of koppelen van volumes in een module-container.  
* **status** – de status waarin de Edge-agent de module plaatst. Deze waarde wordt meestal ingesteld op *met* zoals de meeste mensen wilt gebruiken voor de agent van de rand om direct te starten alle modules op het apparaat. U kan echter opgeven dat de oorspronkelijke status van een module die u wilt stoppen en wachten op een later tijdstip aan de rand agent starten van een module. De agent rand meldt de status van elke module terug naar de cloud in de eigenschappen van gemeld. Een verschil tussen de gewenste eigenschappen en de gerapporteerde is een indicator of een apparaat zorgt. De ondersteunde statussen zijn:
   * Downloaden
   * Actief
   * Niet in orde
   * Mislukt
   * Stopped
* **restartPolicy** : hoe de Edge-agent opnieuw wordt opgestart een module. Mogelijke waarden:
   * Nooit: Start de agent rand nooit de module.
   * onFailure - als de module vastloopt, de Edge-agent opnieuw worden opgestart. Als de module nu correct wordt afgesloten, de Edge-agent niet opnieuw wordt opgestart deze.
   * Niet in orde - als de module vastloopt of geacht beschadigd, de Edge-agent start deze opnieuw op.
   * Altijd - als de module is vastgelopen, wordt geacht slecht of op een manier is afgesloten, de Edge-agent opnieuw worden opgestart. 

Rand van de IoT-agent verzendt runtime-antwoord naar IoT Hub. Hier volgt een lijst met mogelijke reacties:
  * 200 - OK
  * 400 - configuratie van de implementatie is onjuist gevormd of ongeldig.
  * 417 - het apparaat heeft geen een implementatieconfiguratie ingesteld.
  * 412 - de schemaversie in de configuratie van de implementatie is ongeldig.
  * 406 - het edge-apparaat is offline of niet verzenden statusrapporten.
  * 500 - een fout opgetreden in de edge-runtime.

### <a name="security"></a>Beveiliging

De agent IoT rand speelt een cruciale rol in de beveiliging van een Edge van de IoT-apparaat. Bijvoorbeeld, voert acties zoals het controleren van de installatiekopie van een module voordat u deze uit. Deze functies worden algemene beschikbaarheid van V2-functies toegevoegd. 

<!-- For more information about the Azure IoT Edge security framework, see []. -->

## <a name="next-steps"></a>Volgende stappen

- [Azure IoT rand modules begrijpen][lnk-modules]

<!-- Images -->
[1]: ./media/iot-edge-runtime/pipeline.png
[2]: ./media/iot-edge-runtime/gateway.png
[3]: ./media/iot-edge-runtime/ModuleEndpoints.png
[4]: ./media/iot-edge-runtime/ModuleEndpointsWithRoutes.png

<!-- Links -->
[lnk-modules]: iot-edge-modules.md