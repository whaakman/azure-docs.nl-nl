---
title: Inzicht in de Azure IoT Edge-runtime | Microsoft Docs
description: Meer informatie over de Azure IoT Edge-runtime en hoe deze in staat stelt uw edge-apparaten
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 08/13/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 51f00b46283adf0f64bf37d5813640aa4e36f667
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/12/2018
ms.locfileid: "51567244"
---
# <a name="understand-the-azure-iot-edge-runtime-and-its-architecture"></a>De Azure IoT Edge-runtime en de bijbehorende architectuur begrijpen

IoT Edge-runtime is een verzameling van programma's die moeten worden geïnstalleerd op een apparaat om te worden beschouwd als een IoT Edge-apparaat. Gezamenlijk, de onderdelen van de IoT Edge-runtime voor het ontvangen van code worden uitgevoerd aan de rand IoT Edge-apparaten inschakelen en kennis van de resultaten. 

IoT Edge-runtime voert de volgende functies op IoT Edge-apparaten:

* Installeert workloads op het apparaat en werkt deze bij.
* Onderhoudt de Azure IoT Edge-beveiligingsstandaarden op het apparaat.
* Zorgt ervoor dat [IoT Edge-modules](iot-edge-modules.md) altijd worden uitgevoerd.
* Rapporteert de status van de module aan de cloud voor externe bewaking.
* Vergemakkelijkt de communicatie tussen downstream leaf-apparaten en IoT Edge-apparaten.
* Vergemakkelijkt de communicatie tussen modules op het IoT Edge-apparaat.
* Vergemakkelijkt de communicatie tussen het IoT Edge-apparaat en de cloud.

![IoT Edge-runtime communiceert inzichten en status van de module voor IoT-Hub](./media/iot-edge-runtime/Pipeline.png)

De verantwoordelijkheden van de IoT Edge-runtime kunnen worden onderverdeeld in twee categorieën: communicatie-en-module. Deze twee rollen worden uitgevoerd door twee onderdelen die gezamenlijk de IoT Edge-runtime. De IoT Edge hub is verantwoordelijk voor communicatie, terwijl de IoT Edge-agent wordt beheerd, implementeren en controleren van de modules. 

Zowel de Edge hub en de Edge agent zijn modules, net als elke andere module die wordt uitgevoerd op een IoT Edge-apparaat. 

## <a name="iot-edge-hub"></a>IoT Edge hub

De Edge hub is een van twee modules die gezamenlijk de Azure IoT Edge-runtime. Het fungeert als een lokale proxyserver voor IoT-Hub bij het blootstellen van de dezelfde protocoleindpunten als IoT-Hub. Deze consistentie betekent dat clients (of apparaten of modules) kan verbinding maken met de IoT Edge-runtime, net als met IoT Hub. 

>[!NOTE]
>Edge Hub biedt ondersteuning voor clients die verbinding maken met behulp van MQTT- of AMQP. Clients die HTTP gebruiken, worden niet ondersteund. 

De Edge hub is niet een volledige versie van IoT-Hub die lokaal wordt uitgevoerd. Er zijn enkele dingen die Edge hub op de achtergrond naar IoT Hub delegeert. Edge hub verzendt bijvoorbeeld verificatieaanvragen naar IoT-Hub wanneer een apparaat de eerste keer probeert om verbinding te maken. Nadat de eerste verbinding tot stand is gebracht, wordt informatie over beveiliging lokaal cache door Edge hub. Volgende verbindingen vanaf dat apparaat toegestaan zonder dat om te verifiëren naar de cloud. 

>[!NOTE]
>De runtime moet worden verbonden telkens wanneer er wordt geprobeerd om een apparaat te verifiëren.

Als u wilt de bandbreedte reduceren uw IoT Edge-oplossing gebruikt, de Edge hub optimaliseert het aantal daadwerkelijke verbindingen worden aangebracht in de cloud. Edge hub neemt logische verbindingen van clients, zoals modules of leaf-apparaten en worden ze gecombineerd voor één fysieke verbinding naar de cloud. De details van dit proces zijn transparant voor de rest van de oplossing. U kunt clients zien dat ze hun eigen verbinding naar de cloud hebben, zelfs als ze al worden verzonden via dezelfde verbinding. 

![Edge hub fungeert als een gateway tussen meerdere fysieke apparaten en de cloud](./media/iot-edge-runtime/Gateway.png)

Edge hub kunt bepalen of deze verbonden met IoT Hub. Als de verbinding verbroken wordt, Edge hub-berichten of dubbele updates lokaal wordt opgeslagen. Zodra een verbinding opnieuw tot stand is gebracht, worden deze gesynchroniseerd met alle gegevens. De locatie die wordt gebruikt voor deze tijdelijke cache wordt bepaald door een eigenschap van de moduledubbel van de Edge hub. De grootte van de cache wordt niet beperkt tot, en zullen groeien zolang het apparaat heeft opslagcapaciteit. 

### <a name="module-communication"></a>Module-communicatie

Edge Hub vergemakkelijkt de communicatie van de module-module. Edge Hub gebruikt als een berichtenbroker, houdt modules onafhankelijk van elkaar. Modules hoeft alleen te geven van de invoer waarop ze berichten en de uitvoer waaraan ze berichten schrijven accepteren. Een ontwikkelaar van de oplossing vervolgens dan aan elkaar gehecht deze invoer en uitvoer samen zodat de modules verwerken van gegevens in de volgorde die specifiek zijn voor die oplossing. 

![Edge Hub vergemakkelijkt de communicatie van de module-naar-module](./media/iot-edge-runtime/ModuleEndpoints.png)

Een module aanroepen om gegevens te verzenden naar de Edge hub, de methode SendEventAsync. Het eerste argument geeft op welke uitvoer het bericht te verzenden. De volgende pseudocode verzendt een bericht op output1:

   ```csharp
   ModuleClient client = new ModuleClient.CreateFromEnvironmentAsync(transportSettings); 
   await client.OpenAsync(); 
   await client.SendEventAsync(“output1”, message); 
   ```

Registreren voor het ontvangen van een bericht, een callback die de berichten die binnenkomen op een specifieke invoer worden verwerkt. De volgende pseudocode registreert de messageProcessor functie moet worden gebruikt voor het verwerken van alle berichten ontvangen op input1:

   ```csharp
   await client.SetEventHandlerAsync(“input1”, messageProcessor, userContext);
   ```

De ontwikkelaar van de oplossing is verantwoordelijk voor het opgeven van de regels die bepalen hoe Edge hub wordt doorgegeven berichten tussen modules. Regels voor doorsturen zijn gedefinieerd in de cloud en naar Edge hub in de apparaatdubbel verlaagd. Dezelfde syntaxis voor routes voor IoT Hub wordt gebruikt voor het definiëren van routes tussen modules in Azure IoT Edge. 

<!--- For more info on how to declare routes between modules, see []. --->   

![Routes tussen modules](./media/iot-edge-runtime/ModuleEndpointsWithRoutes.png)

## <a name="iot-edge-agent"></a>IoT Edge-agent

De IoT Edge-agent is de module die de Azure IoT Edge-runtime vormt. Het is verantwoordelijk voor het instantiëren van modules, ervoor te zorgen dat ze worden uitgevoerd en de status van de modules teruggestuurd rapportages naar IoT Hub. Net als elke andere module gebruikt de Edge agent de moduledubbel voor het opslaan van deze configuratiegegevens. 

De [IoT Edge security daemon](iot-edge-security-manager.md) wordt de Edge agent wordt gestart bij het opstarten van apparaat. De agent haalt de moduledubbel van IoT-Hub en inspecteert het manifest van de implementatie. Het manifest voor de implementatie is een JSON-bestand dat verklaart de modules die worden gestart. 

Elk item in het manifest van de implementatie bevat specifieke informatie over een module en door de Edge agent wordt gebruikt voor het beheren van de levenscyclus van de module. Sommige van de interessanter eigenschappen zijn: 

* **Settings.Image** – de container-installatiekopie die gebruikmaakt van de Edge agent te starten van de module. De Edge agent moet worden geconfigureerd met referenties voor de container registry als de afbeelding met een wachtwoord is beveiligd. Referenties voor het containerregister dat kan worden geconfigureerd op afstand met behulp van de implementatie van het manifest, of op het Edge-apparaat zelf door bij te werken de `config.yaml` bestand in de map IoT Edge-programma.
* **settings.createOptions** : een tekenreeks is die rechtstreeks naar de Docker-daemon wordt doorgegeven bij het starten van een module-container. Docker-opties toe te voegen in deze eigenschap kunt voor geavanceerde opties, zoals poort doorsturen of koppelen van volumes in de container van een module.  
* **status** – de status waarin de Edge agent de module plaatst. Deze waarde wordt meestal ingesteld op *met* als de meeste mensen willen de Edge agent om direct te starten alle modules op het apparaat. U kan echter de initiële status van een module om te worden gestopt en wachten op een later tijdstip te zien van de Edge-agent te starten van een module opgeven. De Edge agent rapporteert de status van elke module terug naar de cloud in de gerapporteerde eigenschappen. Een verschil tussen de gewenste eigenschap en de gerapporteerde eigenschap is een indicator van een metagegevenscaching apparaat. De ondersteunde statussen zijn:
   * Downloaden
   * In uitvoering
   * Niet in orde
   * Mislukt
   * Gestopt
* **restartPolicy** : de manier waarop de Edge agent opnieuw wordt opgestart een module. Mogelijke waarden:
   * Nooit: Start de Edge agent nooit de module.
   * onFailure - als de module vastloopt, de Edge agent opnieuw worden opgestart. Als de module nu foutloos wordt afgesloten, de Edge agent niet opnieuw wordt opgestart deze.
   * Niet in orde - als de module vastloopt of wordt beschouwd als niet in orde, de Edge agent start deze opnieuw op.
   * Altijd - als de module vastloopt, niet in orde wordt beschouwd, of op geen enkele manier worden afgesloten, de Edge agent opnieuw worden opgestart. 

De IoT Edge-agent verzendt runtimereactie naar IoT Hub. Hier volgt een lijst van mogelijke reacties:
  * 200 - OK
  * 400 - de implementatieconfiguratie is onjuist gevormd of ongeldig.
  * 417 - het apparaat heeft geen een implementatieconfiguratie instellen.
  * 412 - de schemaversie in de implementatieconfiguratie is ongeldig.
  * 406 - het edge-apparaat is offline of verzendt geen statusrapporten.
  * 500 - een fout opgetreden in de edge-runtime.

### <a name="security"></a>Beveiliging

De IoT Edge agent speelt een cruciale rol in de beveiliging van een IoT Edge-apparaat. Bijvoorbeeld, worden er acties, zoals de installatiekopie van een module controleren voordat u begint met het uitgevoerd. 

Voor meer informatie over het Azure IoT Edge security-framework, leest u over de [IoT Edge-beveiligingsbeheer](iot-edge-security-manager.md)

## <a name="next-steps"></a>Volgende stappen

[Informatie over Azure IoT Edge-certificaten](iot-edge-certs.md)
