---
title: Azure IoT Hub apparaat-streams (preview) | Microsoft Docs
description: Overzicht van IoT Hub apparaat-streams.
author: rezasherafat
manager: briz
services: iot-hub
ms.service: iot-hub
ms.topic: conceptual
ms.date: 01/15/2019
ms.author: rezas
ms.openlocfilehash: 7b436acb937216ad02289282f4a7ac5f88df2eea
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57245145"
---
# <a name="iot-hub-device-streams-preview"></a>IoT Hub apparaat-Streams (preview)

## <a name="overview"></a>Overzicht
Azure IoT Hub *apparaat streams* het maken van veilige bidirectionele TCP-tunnels voor een verscheidenheid aan scenario's voor cloud-naar-apparaat-communicatie te vergemakkelijken. Een stream van het apparaat wordt via een IoT-hub *streaming-eindpunt* die fungeert als een proxy tussen uw apparaat en service-eindpunten. Deze instelling, die in het onderstaande diagram is vooral nuttig wanneer apparaten zich achter een firewall van het netwerk of zich binnen een particulier netwerk bevinden. Als zodanig kunnen IoT Hub apparaat-streams adres klanten moet worden bereikt van IoT-apparaten op een manier firewall-vriendelijk en zonder de noodzaak voor het openen van ruim netwerkpoorten binnenkomende of uitgaande firewall.

![Alternatieve tekst](./media/iot-hub-device-streams-overview/iot-hub-device-streams-overview.png "IoT Hub device streamt overzicht")


Apparaten met behulp van IoT Hub apparaat-streams, beveiligd blijven en moeten alleen openen via poort 443 uitgaande TCP-verbindingen met IoT-hub streaming-eindpunt. Nadat een stroom tot stand is gebracht, wordt de servicezijde en apparaat-side '-toepassingen elk hebt programmatische toegang tot een WebSocket-clientobject te verzenden en ontvangen van onbewerkte bytes met elkaar. De betrouwbaarheid en de volgorde van garanties geboden door deze tunnel is gelijk aan TCP.

## <a name="benefits"></a>Voordelen
IoT Hub apparaat stromen bieden de volgende voordelen:
- **Firewall-vriendelijk beveiligde verbindingen:** IoT-apparaten kunnen worden bereikt vanaf het service-eindpunten zonder inkomende firewallpoort worden geopend op het apparaat of de netwerk-verbindingen (alleen uitgaande verbinding met IoT Hub moet via poort 443).

- **Verificatie:** Apparaat- en service zijden van de tunnel moeten zich verifiëren met IoT Hub met behulp van de bijbehorende referenties.

- **Versleuteling:** Standaard wordt met IoT Hub apparaat-streams verbindingen TLS-functionaliteit gebruiken. Dit zorgt ervoor dat het verkeer altijd worden versleuteld, ongeacht of de toepassing gebruikmaakt van versleuteling of niet.

- **De eenvoud van de verbinding:** Het gebruik van apparaat-streams elimineert in veel gevallen de noodzaak voor complexe installatie van het virtuele particuliere netwerken voor het inschakelen van de verbinding met IoT-apparaten.

- **Compatibiliteit met TCP/IP-stack:** IoT Hub apparaat-streams aankan TCP/IP-toepassingsverkeer. Dit betekent dat deze functie kan gebruikmaken van een breed scala van bedrijfseigen, evenals op standaarden gebaseerde protocollen.

- **Het gebruiksgemak gebruikt in een particulier netwerk-instellingen:** Service kan communiceren met een apparaat door te verwijzen naar de apparaat-ID in plaats van de IP-adres van het apparaat. Dit is handig in situaties waarin een apparaat bevindt zich in een particulier netwerk en een privé IP-adres heeft, of het IP-adres dynamisch wordt toegewezen en is onbekend bij de servicezijde.

## <a name="device-stream-workflows"></a>Werkstromen voor Stream van apparaten
Een apparaat-stroom wordt gestart wanneer de service vraagt om verbinding maken met een apparaat door op te geven van de apparaat-ID. Deze werkstroom is met name in een client/server-communicatiemodel, met inbegrip van SSH en RDP, waarbij een gebruiker zal worden op afstand verbinding maken met de SSH of RDP-server die wordt uitgevoerd op het apparaat met behulp van een SSH of RDP-clientprogramma past.

De apparaat-proces voor het maken van stream omvat een onderhandeling tussen het apparaat, service, de belangrijkste IoT-hub en streaming-eindpunten. Terwijl de belangrijkste eindpunt van de IoT hub het maken van een apparaat-stream coördineert, verwerkt het streaming-eindpunt het verkeer tussen de service en het apparaat.

### <a name="device-stream-creation-flow"></a>Apparaat stream het maken van flow
Programmatische maken van een apparaat-stream met behulp van de SDK omvat de volgende stappen, die ook worden weergegeven in de afbeelding hieronder:

![Alternatieve tekst](./media/iot-hub-device-streams-overview/iot-hub-device-streams-handshake.png "apparaat stream handshake-proces")


1. De apparaattoepassing wordt geregistreerd voor een retouraanroep vooraf om te worden geïnformeerd over wanneer een nieuw apparaat-stroom wordt gestart op het apparaat. Deze stap vindt doorgaans plaats wanneer het apparaat wordt opgestart en verbinding met IoT Hub maakt.

2. Het programma aan de serverkant initieert de stroom van een apparaat wanneer dit nodig is door te geven van de apparaat-ID (_niet_ het IP-adres).

3. IoT-hub ontvangt een melding van het programma voor apparaat door het aanroepen van de callback geregistreerd in stap 1. Het apparaat kan accepteren of afwijzen van de aanvraag van de initialisatie van stream. Deze logica kan zijn specifiek is voor uw toepassingsscenario zijn. Als de streamaanvraag is afgewezen door het apparaat, informeert IoT Hub de service. vervolg anders met de onderstaande stappen.

4. Het apparaat een beveiligde uitgaande TCP-verbinding met het streaming-eindpunt maakt via poort 443 en de verbinding wordt bijgewerkt naar een WebSocket. De URL van het streaming-eindpunt, evenals de referenties gebruiken om te verifiëren beide vindt u op het apparaat met IoT Hub als onderdeel van de aanvraag verzonden in stap 3.

5. De service is een melding ontvangt als het resultaat van het apparaat de stream accepteren en gaat deze naar een eigen WebSocket-client om het streaming-eindpunt te maken. Op deze manier wordt de streaming eindpunt-URL en verificatie gegevens ontvangen van IoT-Hub.

In de bovenstaande handshake:
- De handshake moet voltooien binnen 60 seconden (stap 2 t/m 5), anders wordt de handshake is mislukt met een time-out en de service zal worden op de hoogte gesteld.

- Nadat de bovenstaande stream maken stroom is voltooid, wordt het streaming-eindpunt fungeert als een proxy en wordt verkeer overdragen tussen de service en het apparaat via hun respectieve WebSockets.

- Apparaten en services moeten uitgaande connectiviteit voor de belangrijkste eindpunt van de IoT Hub, evenals het streaming-eindpunt via poort 443. De URL van deze eindpunten is beschikbaar op *overzicht* tabblad in de portal van de IoT-Hub.

- De betrouwbaarheid en het waarborgen van een stroom tot stand gebrachte bestellen is gelijk aan TCP.

- Alle verbindingen met IoT Hub en streaming-eindpunt gebruik van TLS en worden versleuteld.

### <a name="termination-flow"></a>Flow beëindigen
Een bestaande stream wordt beëindigd wanneer een van de TCP-verbindingen met de gateway niet zijn verbonden (door de service of het apparaat). Dit kan plaatsvinden vrijwillig door te sluiten van de WebSocket op het apparaat of de service-programma's of onopzettelijk in het geval van een netwerk-out- of proces verbindingsfout. Na beëindiging van het apparaat of van de service verbinding met het streaming-eindpunt, de TCP-verbinding wordt ook (geforceerd) beëindigd en de service en het apparaat zijn verantwoordelijk voor de stroom opnieuw te maken indien nodig.


## <a name="connectivity-requirements"></a>Vereisten voor connectiviteit

Het apparaat en de service-zijde van de stroom van een apparaat moet kunnen tot stand brengen van TLS-functionaliteit verbindingen met IoT Hub en de streaming-eindpunt. Hiervoor moet uitgaande connectiviteit via poort 443 met deze eindpunten. De hostnaam die is gekoppeld aan deze eindpunten kunt u vinden op de *overzicht* tabblad van IoT-Hub, zoals wordt weergegeven in de afbeelding hieronder: ![Alternatieve tekst](./media/iot-hub-device-streams-overview/device-stream-portal.png "apparaat stream eindpunten")

U kunt ook de informatie van de eindpunten kan gebruik worden opgehaald met behulp van Azure CLI onder de sectie met eigenschappen van de hub, met name `property.hostname` en `property.deviceStreams` sleutels.

```azurecli-interactive
az iot hub devicestream show --name <YourIoTHubName>
```

De uitvoer is een JSON-object van alle eindpunten die wellicht verbinden om te kunnen maken van een apparaat-stream van uw hub apparaat- en service.

```json
{
  "streamingEndpoints": [
    "https://<YourIoTHubName>.<region-stamp>.streams.azure-devices.net"
  ]
}
```

> [!NOTE]
> Zorg ervoor dat u Azure CLI versie 2.0.57 hebt geïnstalleerd of hoger. U kunt de nieuwste versie [hier](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) downloaden.
> 

## <a name="whitelist-device-streaming-endpoints"></a>Lijst met toegestane adressen apparaat Streaming-eindpunten

Zoals vermeld [eerdere](#Overview), het apparaat maakt een uitgaande verbinding naar IoT Hub-streaming-eindpunt tijdens apparaat stromen in gang zetten. Uw firewalls op het apparaat of het netwerk moeten uitgaande connectiviteit voor de streaming-gateway via poort 443 (Houd er rekening mee dat de communicatie plaatsvindt via een WebSocket-verbinding die is versleuteld met behulp van TLS) toestaan.

De hostnaam van apparaat-streaming-eindpunt kan worden gevonden op de Azure IoT Hub-portal onder het tabblad Overzicht. ![Alternatieve tekst](./media/iot-hub-device-streams-overview/device-stream-portal.PNG "apparaat stream eindpunten")

U vindt hier ook deze informatie met behulp van Azure CLI:

```azurecli-interactive
az iot hub devicestream show --name <YourIoTHubName>
```

> [!NOTE]
> Zorg ervoor dat u Azure CLI versie 2.0.57 hebt geïnstalleerd of hoger. U kunt de nieuwste versie [hier](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) downloaden.
> 

## <a name="troubleshoot-via-device-streams-activity-logs"></a>Problemen oplossen via apparaat Streams activiteitenlogboeken

U kunt de logboeken van Azure Monitor instellen voor het verzamelen van het activiteitenlogboek van apparaat stromen in uw IoT-Hub. Dit kan zeer nuttig zijn bij het oplossen van scenario's zijn.

Volg onderstaande stappen voor het configureren van Azure Monitor-logboeken voor uw IoT-Hub apparaat stream activiteiten:

1. Navigeer naar de *diagnostische instellingen* tabblad in uw IoT-Hub en klik op *diagnostische gegevens inschakelen* koppeling.

  ![Alternatieve tekst](./media/iot-hub-device-streams-overview/device-streams-diagnostics-settings.PNG "disgnostics Logboeken inschakelen")


2. Geef een naam op voor de diagnostische instellingen en kies *verzenden naar Log Analytics* optie. Training voor het kiezen van een bestaande resource van de Log Analytics-werkruimte of maak een nieuwe. Controleer daarnaast de *DeviceStreams* in de lijst.

    ![Alternatieve tekst](./media/iot-hub-device-streams-overview/device-streams-diagnostics.PNG "inschakelen apparaat streamt Logboeken")

3. U hebt nu toegang tot uw apparaat stromen Logboeken onder de *logboeken* tabblad in de portal van uw IoT-Hub. Stream-activiteitenlogboeken apparaat wordt weergegeven in de `AzureDiagnostics` tabel en hebben `Category=DeviceStreams`.

    <p>
De identiteit van het doelapparaat en het resultaat van de bewerking is ook beschikbaar in de logboeken zoals hieronder wordt weergegeven.
    ![Alternatieve tekst](./media/iot-hub-device-streams-overview/device-streams-log-analytics.PNG "apparaat stream Logboeken openen")


## <a name="regional-availability"></a>Regionale beschikbaarheid

Tijdens de openbare preview zijn er in de regio's VS-midden en centrale EUAP ons IoT Hub apparaat stromen beschikbaar. Zorg ervoor dat u uw hub maken in een van deze regio's. 


## <a name="sdk-availability"></a>SDK-beschikbaarheid

Beide zijden van elke stroom (aan de apparaat- en service) gebruiken de SDK voor IoT Hub tot stand brengen van de tunnel. Klanten kunnen kiezen tijdens de openbare preview van de volgende SDK-talen:
- De C en C# SDK's ondersteunen apparaat stromen op het apparaat plaats.

- De NodeJS en C# SDK ondersteuning voor apparaat stromen aan de servicezijde.


## <a name="iot-hub-device-stream-samples"></a>IoT Hub apparaat Stream-voorbeelden

We hebben twee gepubliceerd [snelstartgids voorbeelden](/azure/iot-hub) ter illustratie van het gebruik van apparaat stromen door toepassingen.
* De *echo* voorbeeld ziet u programmatische gebruik van apparaat-streams (door de SDK-API voor het rechtstreeks aanroepen).
* De *lokale proxy* voorbeeld demonstreert de tunneling van verkeer van gebruiksklare client/server-toepassing (zoals SSH, RDP of op het web) via apparaat stromen.

Deze voorbeelden worden hieronder nader besproken.

### <a name="echo-sample"></a>Echo-voorbeeld
De echo-voorbeeld ziet u programmatische gebruik van stromen voor verzenden en ontvangen bytes tussen service- en apparaattoepassingen apparaat. Gebruik de onderstaande koppelingen voor toegang tot de Quick Start-handleidingen. Houd er rekening mee dat u service- en programma's kunt gebruiken in verschillende talen, zoals C apparaatprogramma kunt werken met C# programma van de service.

| SDK    | Serviceprogramma                                          | Apparaatprogramma                                           |
|--------|----------------------------------------------------------|----------------------------------------------------------|
| C#     | [Link](quickstart-device-streams-echo-csharp.md) | [Link](quickstart-device-streams-echo-csharp.md) |
| Node.js | [Link](quickstart-device-streams-echo-nodejs.md) | -                                                        |
| C      | -                                                        | [Link](quickstart-device-streams-echo-c.md)      |

### <a name="local-proxy-sample-for-ssh-or-rdp"></a>Voorbeeld van een lokale proxyserver (voor SSH of RDP)
Het lokale proxy-voorbeeld ziet u een manier om in te schakelen tunneling van een bestaande toepassing verkeer dat betrekking heeft op de communicatie tussen een client en een server-programma. Deze set up geschikt is voor client/server-protocollen zoals SSH en RDP, waarbij de servicezijde fungeert als een client (met SSH of RDP-client-programma's) en de apparaat-zijde fungeert als de server (met SSH-daemon of RDP serverprogramma's). 

Deze sectie beschrijft het gebruik van apparaat-streams zodat de gebruiker SSH naar een apparaat via de apparaat-streams (het geval is bij RDP- of andere client/server-toepassingen zijn vergelijkbaar met behulp van het protocol overeenkomende poort).

De installatie maakt gebruik van twee *lokale proxy* programma's die worden weergegeven in de afbeelding hieronder, namelijk *apparaat lokale proxy* en *proxy van service-lokale*. De lokale proxy-programma's zijn verantwoordelijk voor het uitvoeren van de [apparaat stream inleiding handshake](#device-stream-creation-flow) met IoT Hub en interactie met SSH-client en SSH-daemon met behulp van reguliere client/server-sockets.

![Alternatieve tekst](./media/iot-hub-device-streams-overview/iot-hub-device-streams-ssh.png "apparaat stream proxy-instellingen voor SSH/RDP")

1. De gebruiker wordt uitgevoerd voor service-local-proxy voor het starten van de stroom van een apparaat op het apparaat.

2. De apparaat-local-proxy worden stream het initiëren van aanvragen geaccepteerd en de tunnel is ingesteld op IoT-Hub streaming-eindpunt (zoals hierboven wordt beschreven).

3. De apparaat-local-proxy verbinding met het eindpunt van de SSH-daemon luistert op poort 22 op het apparaat.

4. De proxy-service-lokale luistert naar een aangewezen poort die in afwachting van nieuwe SSH-verbindingen van de gebruiker (poort 2222 die wordt gebruikt in het voorbeeld, maar dit kan worden geconfigureerd om een beschikbare poort). De gebruiker wijst de SSH-client naar de service-local-proxypoort op localhost.

### <a name="notes"></a>Opmerkingen
- De bovenstaande stappen voltooien een end-to-end-tunnel tussen de SSH-client (aan de rechterkant) naar de SSH-daemon (aan de linkerkant). Onderdeel van deze end-to-end-connectiviteit omvat het verkeer via de stroom van een apparaat verzenden naar IoT Hub.

- De pijlen in de afbeelding geven de richting waarin verbindingen tot stand tussen de eindpunten gebracht worden. Specifiek, houd er rekening mee dat er geen binnenkomende verbindingen naar het apparaat (dit wordt vaak geblokkeerd door een firewall).

- De keuze van het gebruik van poort 2222 op de service-local-proxy is een willekeurige keuze. De proxy kan worden geconfigureerd voor het gebruik van een beschikbare poort.

- De keuze van poort 22 is afhankelijk van het procotocol en specifiek voor SSH in dit geval. Voor het geval van RDP, moet de poort 3389 worden gebruikt. Dit kan worden geconfigureerd in de opgegeven voorbeeld programma's.

Gebruik de onderstaande koppelingen voor instructies over het uitvoeren van de lokale proxy's in uw taal naar keuze. Vergelijkbaar met de [echo voorbeeld](#echo-sample), kunt u apparaat - en service-lokale proxy-programma's uitvoeren in verschillende talen als ze volledig compatibel zijn.

| SDK    | Service-Local Proxy                                       | Apparaat-Local-Proxy                                |
|--------|-----------------------------------------------------------|---------------------------------------------------|
| C#     | [Link](quickstart-device-streams-proxy-csharp.md)  | [Link](quickstart-device-streams-proxy-csharp.md) |
| Node.js | [Link](quickstart-device-streams-proxy-nodejs.md)         | -                                                 |
| C      | -                                                         | [Link](quickstart-device-streams-proxy-c.md)      |

## <a name="next-steps"></a>Volgende stappen

Gebruik de onderstaande koppelingen voor meer informatie over apparaatstreams:

> [!div class="nextstepaction"]
> [Apparaat-streams op IoT weergeven (Channel 9)](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fchannel9.msdn.com%2FShows%2FInternet-of-Things-Show%2FAzure-IoT-Hub-Device-Streams&data=02%7C01%7Crezas%40microsoft.com%7Cc3486254a89a43edea7c08d67a88bcea%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636831125031268909&sdata=S6u9qiehBN4tmgII637uJeVubUll0IZ4p2ddtG5pDBc%3D&reserved=0)
> [probeert de Snelstartgids van een apparaat-stream](/azure/iot-hub)
