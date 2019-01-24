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
ms.openlocfilehash: 7ffe4a087ae94d6c96019cc045d3d7ff071780d4
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54830352"
---
# <a name="iot-hub-device-streams-preview"></a>IoT Hub apparaat-Streams (preview)

## <a name="overview"></a>Overzicht
Azure IoT Hub *apparaat streams* het maken van veilige bidirectionele TCP-tunnels voor een verscheidenheid aan scenario's voor cloud-naar-apparaat-communicatie te vergemakkelijken. Een stream van het apparaat wordt via een IoT-hub *streaming-eindpunt* die fungeert als een proxy tussen uw apparaat en service-eindpunten. Deze instelling wordt weergegeven in het diagram hieronder espcially nuttig is wanneer apparaten zich achter een netwerkfirewall of zich binnen een particulier netwerk bevinden. Als zodanig kunnen IoT Hub apparaat-streams adres klanten moet worden bereikt van IoT-apparaten op een manier firewall-vriendelijk en zonder de noodzaak voor het openen van ruim netwerkpoorten binnenkomende of uitgaande firewall.

![Alternatieve tekst](./media/iot-hub-device-streams-overview/iot-hub-device-streams-overview.png "IoT Hub device streamt overzicht")


Apparaten met behulp van IoT Hub apparaat-streams, beveiligd blijven en moeten alleen openen via poort 443 uitgaande TCP-verbindingen met IoT-hub streaming-eindpunt. Zodra een stroom tot stand is gebracht, hebben de servicezijde en apparaat-side '-toepassingen elke programmatische toegang tot een WebSocket-clientobject voor het verzenden en ontvangen van ruwe byte met elkaar. De betrouwbaarheid en de volgorde van garanties geboden door deze tunnel is gelijk aan TCP.

## <a name="benefits"></a>Voordelen
IoT Hub apparaat stromen bieden de volgende voordelen:
- **Firewall-vriendelijk beveiligde verbindingen:** IoT-apparaten kunnen worden bereikt vanaf het service-eindpunten zonder inkomende firewallpoort worden geopend op het apparaat of de netwerk-verbindingen (alleen uitgaande verbinding met IoT Hub moet via poort 443).

- **Verificatie:** Apparaat- en service zijden van de tunnel moeten zich verifiëren met IoT Hub met behulp van de bijbehorende referenties.

- **Versleuteling:** Standaard wordt met IoT Hub apparaat-streams verbindingen TLS-functionaliteit gebruiken. Dit zorgt ervoor dat het verkeer altijd worden versleuteld, ongeacht of de toepassing gebruikmaakt van versleuteling of niet.

- **De eenvoud van de verbinding:** Het gebruik van apparaat-streams elimineert de noodzaak voor complexe installatie van het virtuele particuliere netwerken voor het inschakelen van de verbinding met IoT-apparaten.

- **Compatibiliteit met TCP/IP-stack:** IoT Hub apparaat-streams aankan TCP/IP-toepassingsverkeer. Dit betekent dat deze functie kan gebruikmaken van een breed scala van bedrijfseigen, evenals op standaarden gebaseerde protocollen.

- **Het gebruiksgemak gebruikt in een particulier netwerk-instellingen:** Service kan een apparaat bereiken door te verwijzen naar de apparaat-ID in plaats van IP-adres. Dit is handig in situaties waarin een apparaat bevindt zich in een particulier netwerk en een privé IP-adres heeft, of het IP-adres dynamisch wordt toegewezen en is onbekend bij de servicezijde.

## <a name="device-stream-workflows"></a>Werkstromen voor Stream van apparaten
Een apparaat-stroom wordt gestart wanneer de service vraagt om verbinding maken met een apparaat door op te geven van de apparaat-ID. Deze werkstroom is met name past van client/server-communicatiepatroon, met inbegrip van SSH en RDP waar een gebruiker zal worden op afstand verbinding maken met de SSH of RDP-server die wordt uitgevoerd op het apparaat via een programma voor SSH of RDP-client.

De apparaat-proces voor het maken van stream omvat een onderhandeling tussen het apparaat, service, de belangrijkste IoT-hub en streaming-eindpunten. Terwijl de belangrijkste eindpunt van de IoT hub het maken van een apparaat-stream coördineert, verwerkt het streaming-eindpunt het verkeer tussen de service en het apparaat.

### <a name="device-stream-creation-flow"></a>Apparaat stream het maken van flow
Programmatische maken van een apparaat-stream met behulp van de SDK omvat de volgende stappen, die ook worden weergegeven in de afbeelding hieronder:

![Alternatieve tekst](./media/iot-hub-device-streams-overview/iot-hub-device-streams-handshake.png "apparaat stream handshake-proces")


1. De apparaattoepassing wordt geregistreerd voor een retouraanroep vooraf om te worden geïnformeerd over wanneer een nieuw apparaat-stroom wordt gestart op het apparaat. Deze stap vindt doorgaans plaats wanneer het apparaat wordt opgestart en verbinding met IoT Hub maakt.

2. Het programma aan de serverkant initieert de stroom van een apparaat wanneer dit nodig is door te geven van de apparaat-ID (_niet_ het IP-adres).

3. IoT-hub ontvangt een melding van het programma voor apparaat door het aanroepen van de callback geregistreerd in stap 1. Het apparaat kan accepteren of afwijzen van de aanvraag van de initialisatie van stream. Deze logica kan zijn specifiek is voor uw toepassingsscenario zijn. Als de streamaanvraag is afgewezen door het apparaat, informeert IoT Hub de service. vervolg anders met de onderstaande stappen.

4. Het apparaat een beveiligde uitgaande TCP-verbinding met het streaming-eindpunt maakt via poort 443 en de verbinding wordt bijgewerkt naar een WebSocket. De URL van het streaming-eindpunt, evenals de referenties gebruiken om te verifiëren beide vindt u op het apparaat met IoT Hub als onderdeel van de aanvraag verzonden in stap 3.

5. De service is een melding ontvangt als het resultaat van het apparaat de stream accepteren en gaat deze naar een eigen WebSocket om het streaming-eindpunt te maken. Op deze manier wordt de streaming eindpunt-URL en verificatie gegevens ontvangen van IoT-Hub.

In de bovenstaande handshake:
- De handshake moet voltooien binnen 60 seconden (stap 2 t/m 5), anders wordt de handshake is mislukt met een time-out en de service zal worden op de hoogte gesteld.

- Nadat de bovenstaande stream maken stroom is voltooid, wordt het streaming-eindpunt fungeert als een proxy en wordt verkeer overdragen tussen de service en het apparaat via hun respectieve WebSockets.

- Apparaten en services moeten uitgaande connectiviteit voor de belangrijkste eindpunt van de IoT Hub, evenals het streaming-eindpunt via poort 443. De URL van deze eindpunten is beschikbaar op het tabblad Overzicht van de IoT-Hub-portal.

- De betrouwbaarheid en het waarborgen van een stroom tot stand gebrachte bestellen is gelijk aan TCP.

- Alle verbindingen met IoT Hub en streaming-eindpunt gebruik van TLS en worden versleuteld.

### <a name="termination-flow"></a>Flow beëindigen
Een bestaande stream wordt beëindigd wanneer een van de TCP-verbindingen met de gateway niet zijn verbonden (door de service of het apparaat). Dit kan plaatsvinden vrijwillig door te sluiten van de WebSocket op het apparaat of de service-programma's of onopzettelijk in het geval van een netwerk-out- of proces verbindingsfout. Na beëindiging van het apparaat of van de service verbinding met het streaming-eindpunt, de TCP-verbinding wordt ook (geforceerd) beëindigd en de service en het apparaat zijn verantwoordelijk voor de stroom opnieuw te maken indien nodig.


## <a name="connectivity-requirements"></a>Vereisten voor connectiviteit

Het apparaat en de service-zijde van de stroom van een apparaat moet kunnen tot stand brengen van TLS-functionaliteit verbindingen met IoT Hub en de streaming-eindpunt. Hiervoor moet uitgaande connectiviteit via poort 443 met deze eindpunten. De hostnaam die is gekoppeld aan deze eindpunten kunt u vinden op de *overzicht* tabblad van IoT-Hub, zoals wordt weergegeven in de afbeelding hieronder: ![Alternatieve tekst](./media/iot-hub-device-streams-overview/device-stream-portal.png "apparaat stream eindpunten")

U kunt ook de informatie van de eindpunten kan gebruik worden opgehaald met behulp van Azure CLI onder de sectie met eigenschappen van de hub, met name `property.hostname` en `property.deviceStreams` sleutels.

```azurecli-interactive
az iot hub show --name <YourIoTHubName>
```

## <a name="troubleshoot-via-device-streams-activity-logs"></a>Problemen oplossen via apparaat Streams activiteitenlogboeken

U kunt Azure Log Analytics voor het verzamelen van het activiteitenlogboek van stromen in uw IoT Hub apparaat instellen. Dit kan zeer nuttig zijn bij het oplossen van scenario's zijn.

Volg onderstaande stappen voor het configureren van Azure Log Analytics voor uw IoT-Hub apparaat stream activiteiten:

1. Navigeer naar de *diagnostische instellingen* tabblad in uw IoT-Hub en klik op *diagnostische gegevens inschakelen* koppeling.

  ![Alternatieve tekst](./media/iot-hub-device-streams-overview/device-streams-diagnostics-settings.PNG "disgnostics Logboeken inschakelen")


2. Geef een naam op voor de diagnostische instellingen en kies *verzenden naar Log Analytics* optie. U wordt geholpen om een bestaande Log Analytics-resource kiezen of een nieuwe maken. Controleer daarnaast de *DeviceStreams* in de lijst.

    ![Alternatieve tekst](./media/iot-hub-device-streams-overview/device-streams-diagnostics.PNG "inschakelen apparaat streamt Logboeken")

3. U hebt nu toegang tot uw apparaat stromen Logboeken onder de *logboeken* tabblad in de portal van uw IoT-Hub. Stream-activiteitenlogboeken apparaat wordt weergegeven in de `AzureDiagnostics` tabel en hebben `Category=DeviceStreams`.

    <p>
De identiteit van het doelapparaat en het resultaat van de bewerking is ook beschikbaar in de logboeken zoals hieronder wordt weergegeven.
    ![Alternatieve tekst](./media/iot-hub-device-streams-overview/device-streams-log-analytics.PNG "apparaat stream Logboeken openen")
    

## <a name="whitelist-device-streaming-endpoints"></a>Lijst met toegestane adressen apparaat Streaming-eindpunten

Zoals vermeld [eerdere](#Overview), het apparaat maakt een uitgaande verbinding naar IoT Hub-streaming-eindpunt tijdens apparaat stromen in gang zetten. Uw firewalls op het apparaat of het netwerk moeten uitgaande connectiviteit naar de gateway voor streaming toestaan via poort 443 (dit is een WebSocket-verbinding die is versleuteld met TLS).

De hostnaam van apparaat-streaming-eindpunt kan worden gevonden op de Azure IoT Hub-portal onder het tabblad Overzicht. ![Alternatieve tekst](./media/iot-hub-device-streams-overview/device-stream-portal.PNG "apparaat stream eindpunten")

U vindt hier ook deze informatie met behulp van Azure CLI:
```cmd/sh
az iot hub show --name tcpstreaming-preview
```

## <a name="sdk-availability"></a>SDK-beschikbaarheid
Beide zijden van elke stroom (aan de apparaat- en service) gebruiken de SDK voor IoT Hub tot stand brengen van de tunnel. Klanten kunnen kiezen tijdens de openbare preview van de volgende SDK-talen:
- De C en C# SDK's ondersteunen apparaat stromen op het apparaat plaats.

- De NodeJS en C# SDK ondersteuning voor apparaat stromen aan de servicezijde.

## <a name="iot-hub-device-stream-samples"></a>IoT Hub apparaat Stream-voorbeelden
We hebben twee voorbeelden om te demonstreren van het gebruik van apparaat stromen door toepassingen opgenomen. De *echo* voorbeeld ziet u programmatische gebruik van apparaat-streams (door het aanroepen van de SDK-API). De *lokale proxy* voorbeeld, wordt het gebruik van de SDK-functionaliteit voor het verkeer van de tunnel gebruiksklare toepassing (zoals SSH, RDP of op het web) via apparaat stromen.

### <a name="echo-sample"></a>Echo-voorbeeld
De echo-voorbeeld ziet u programmatische gebruik van apparaat-streams verzenden en ontvangen bytes tussen service- en toepassing. Gebruik de onderstaande koppelingen voor toegang tot de snelstartgidsen (u kunt service- en programma's gebruiken in verschillende talen, zoals C apparaatprogramma kunt werken met C# programma van de service):

| SDK    | Serviceprogramma                                          | Apparaatprogramma                                           |
|--------|----------------------------------------------------------|----------------------------------------------------------|
| C#     | [Link](quickstart-device-streams-echo-csharp.md) | [Link](quickstart-device-streams-echo-csharp.md) |
| Node.js | [Link](quickstart-device-streams-echo-nodejs.md) | -                                                        |
| C      | -                                                        | [Link](quickstart-device-streams-echo-c.md)      |

### <a name="local-proxy-sample-for-ssh-or-rdp"></a>Voorbeeld van een lokale proxyserver (voor SSH of RDP)
Het lokale proxy-voorbeeld ziet u een manier om in te schakelen tunneling van een bestaande toepassing verkeer dat betrekking heeft op de communicatie tussen een client en een server-programma. Deze set up geschikt is voor client/server-protocollen zoals SSH en RDP, waarbij de servicezijde fungeert als een client (met SSH of RDP-client-programma's) en de apparaat-zijde fungeert als de server (met SSH-daemon of RDP serverprogramma's). 

Deze sectie beschrijft het gebruik van apparaat-gegevensstromen naar de SSH-scenario's voor een apparaat via de apparaat-streams (het geval is bij RDP- of andere client/server-protocollen zijn vergelijkbaar met behulp van de bijbehorende poort van het protocol) inschakelen.

De installatie maakt gebruik van twee *lokale proxy* programma's die worden weergegeven in de afbeelding hieronder, namelijk *apparaat lokale proxy* en *proxy van service-lokale*. De lokale proxy's zijn verantwoordelijk voor het uitvoeren van de [apparaat stream inleiding handshake](#Device-stream-creation-flow) met IoT Hub en interactie met SSH-client en SSH-daemon met behulp van reguliere client/server-socket programmeren.

![Alternatieve tekst](./media/iot-hub-device-streams-overview/iot-hub-device-streams-ssh.png "apparaat stream proxy-instellingen voor SSH/RDP")

1. De gebruiker wordt uitgevoerd voor service-local-proxy voor het starten van de stroom van een apparaat op het apparaat.

2. Het apparaat accepteert de initialisatie van de stroom en de tunnel is ingesteld op IoT-Hub streaming-eindpunt (zoals hierboven wordt beschreven).

3. De apparaat-local-proxy verbinding met het eindpunt van de SSH-daemon luistert op poort 22 op het apparaat.

4. De proxy-service-lokale luistert naar een aangewezen poort die in afwachting van nieuwe SSH-verbindingen van de gebruiker (poort 2222 gebruikt in het voorbeeld wordt een willekeurige poort). De gebruiker wijst de SSH-client naar de service-local-proxypoort op localhost.

### <a name="notes"></a>Opmerkingen
- De bovenstaande stappen voltooien een end-to-end-tunnel tussen de SSH-client (aan de rechterkant) naar de SSH-daemon (aan de linkerkant). 

- De pijlen in de afbeelding geven de richting waarin verbindingen tot stand tussen de eindpunten gebracht worden. Specifiek, houd er rekening mee dat er geen binnenkomende verbindingen naar het apparaat (dit wordt vaak geblokkeerd door een firewall).

- De keuze van het gebruik van poort `2222` op de service-local-proxy is een willekeurige keuze. De proxy kan worden geconfigureerd voor het gebruik van een beschikbare poort.

- De keuze van de poort `22` afhankelijk zijn van het procotocol en specifiek voor SSH in dit geval is. Voor het geval van RDP, de poort `3389` moet worden gebruikt. Dit kan worden geconfigureerd in de opgegeven voorbeeld programma's.

Gebruik de onderstaande koppelingen voor instructies over het uitvoeren van de lokale proxy's in uw taal naar keuze. Net als bij de echo-voorbeeld, kunt u uitvoeren apparaat - en service-local-proxy's in verschillende talen, omdat ze volledig compatibel zijn.

| SDK    | Service-Local Proxy                                       | Apparaat-Local-Proxy                                |
|--------|-----------------------------------------------------------|---------------------------------------------------|
| C#     | [Link](quickstart-device-streams-proxy-csharp.md)  | [Link](quickstart-device-streams-proxy-csharp.md) |
| Node.js | [Link](quickstart-device-streams-proxy-nodejs.md)         | -                                                 |
| C      | -                                                         | [Link](quickstart-device-streams-proxy-c.md)      |

## <a name="next-steps"></a>Volgende stappen

Gebruik de onderstaande koppelingen voor meer informatie over apparaat stromen:

> [!div class="nextstepaction"]
> [Apparaat-streams op IoT weergeven (Channel 9)](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fchannel9.msdn.com%2FShows%2FInternet-of-Things-Show%2FAzure-IoT-Hub-Device-Streams&data=02%7C01%7Crezas%40microsoft.com%7Cc3486254a89a43edea7c08d67a88bcea%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636831125031268909&sdata=S6u9qiehBN4tmgII637uJeVubUll0IZ4p2ddtG5pDBc%3D&reserved=0)
> [probeert de Snelstartgids van een apparaat-stream](/azure/iot-hub)
