---
title: Azure Service Fabric reverse proxy diagnostics | Microsoft Docs
description: Informatie over het bewaken en onderzoeken van aanvraag in de omgekeerde proxy worden verwerkt.
services: service-fabric
documentationcenter: .net
author: kavyako
manager: vipulm
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 08/08/2017
ms.author: kavyako
ms.openlocfilehash: 1c62d2390709577bfde6225b783642fb55396a6b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-and-diagnose-request-processing-at-the-reverse-proxy"></a>Controle en diagnose van aanvraagverwerking op de omgekeerde proxy

Beginnen met de 5.7 release van Service Fabric, zijn reverse proxy-gebeurtenissen beschikbaar voor de verzameling. De gebeurtenissen zijn beschikbaar in twee kanalen, één met alleen foutgebeurtenissen met betrekking tot het mislukken van de verwerking van aanvragen bij de omgekeerde proxy en het tweede kanaal met uitgebreide gebeurtenissen met de vermeldingen voor geslaagde en mislukte aanvragen.

Raadpleeg [reverse proxy-gebeurtenissen verzamelen](service-fabric-diagnostics-event-aggregation-wad.md#collect-reverse-proxy-events) om in te schakelen bij het verzamelen van gebeurtenissen uit deze kanalen in lokale en Azure Service Fabric-clusters.

## <a name="troubleshoot-using-diagnostics-logs"></a>Problemen oplossen met Logboeken met diagnostische gegevens
Hier volgen enkele voorbeelden over het interpreteren van de algemene fout logboeken die een kan optreden:

1. Omgekeerde proxy retourneert statuscode van antwoord 504 (time-out).

    Een reden hiervoor kan worden veroorzaakt door de service te beantwoorden binnen de time-outperiode van aanvraag is mislukt.
De eerste gebeurtenis onder Logboeken de details van de aanvraag is ontvangen op de omgekeerde proxy. De tweede gebeurtenis geeft aan dat de aanvraag is mislukt tijdens het doorsturen naar de service, vanwege ' Interne fout ERROR_WINHTTP_TIMEOUT = " 

    De nettolading bevat:

    *  **traceId**: deze GUID kan worden gebruikt om alle gebeurtenissen die overeenkomt met één aanvraag correleren. In de onderstaande twee gebeurtenissen, de traceId = **2f87b722-e254-4ac2-a802-fd315c1a0271**, wat ze deel uitmaken van dezelfde aanvraag.
    *  **Aanvraagurl**: de URL (Reverse proxy-URL) waarop de aanvraag is verzonden.
    *  **term**: HTTP-term.
    *  **remoteAddress**: adres van client verzenden van de aanvraag.
    *  **resolvedServiceUrl**: Service-eindpunt-URL waarnaar de binnenkomende aanvraag opgelost is. 
    *  **errorDetails**: als u meer informatie over de fout.

    ```
    {
      "Timestamp": "2017-07-20T15:57:59.9871163-07:00",
      "ProviderName": "Microsoft-ServiceFabric",
      "Id": 51477,
      "Message": "2f87b722-e254-4ac2-a802-fd315c1a0271 Request url = https://localhost:19081/LocationApp/LocationFEService?zipcode=98052, verb = GET, remote (client) address = ::1, resolved service url = Https://localhost:8491/LocationApp/?zipcode=98052, request processing start time =     15:58:00.074114 (745,608.196 MSec) ",
      "ProcessId": 57696,
      "Level": "Informational",
      "Keywords": "0x1000000000000021",
      "EventName": "ReverseProxy",
      "ActivityID": null,
      "RelatedActivityID": null,
      "Payload": {
        "traceId": "2f87b722-e254-4ac2-a802-fd315c1a0271",
        "requestUrl": "https://localhost:19081/LocationApp/LocationFEService?zipcode=98052",
        "verb": "GET",
        "remoteAddress": "::1",
        "resolvedServiceUrl": "Https://localhost:8491/LocationApp/?zipcode=98052",
        "requestStartTime": "2017-07-20T15:58:00.0741142-07:00"
      }
    }

    {
      "Timestamp": "2017-07-20T16:00:01.3173605-07:00",
      ...
      "Message": "2f87b722-e254-4ac2-a802-fd315c1a0271 Error while forwarding request to service: response status code = 504, description = Reverse proxy Timeout, phase = FinishSendRequest, internal error = ERROR_WINHTTP_TIMEOUT ",
      ...
      "Payload": {
        "traceId": "2f87b722-e254-4ac2-a802-fd315c1a0271",
        "statusCode": 504,
        "description": "Reverse Proxy Timeout",
        "sendRequestPhase": "FinishSendRequest",
        "errorDetails": "internal error = ERROR_WINHTTP_TIMEOUT"
      }
    }
    ```

2. Omgekeerde proxy retourneert antwoord-statuscode 404 (niet gevonden). 
    
    Hier volgt een voorbeeld van de gebeurtenis waarbij omgekeerde proxy 404 retourneert omdat het vinden van het overeenkomende service-eindpunt is mislukt.
    De nettolading van belang zijn hier worden gebruikt:
    *  **processRequestPhase**: Hiermee geeft u de fase tijdens het verwerken van aanvraag als de fout is opgetreden, ***TryGetEndpoint*** eenledige tijdens het ophalen van het service-eindpunt te sturen naar. 
    *  **errorDetails**: geeft een lijst van de zoekcriteria voor het eindpunt. Hier kunt u zien dat de listenerName opgegeven = **FrontEndListener** dat de replica endpoint-lijst alleen een listener met de naam bevat **OldListener**.
    
    ```
    {
      ...
      "Message": "c1cca3b7-f85d-4fef-a162-88af23604343 Error while processing request, cannot forward to service: request url = https://localhost:19081/LocationApp/LocationFEService?ListenerName=FrontEndListener&zipcode=98052, verb = GET, remote (client) address = ::1, request processing start time = 16:43:02.686271 (3,448,220.353 MSec), error = FABRIC_E_ENDPOINT_NOT_FOUND, message = , phase = TryGetEndoint, SecureOnlyMode = false, gateway protocol = https, listenerName = FrontEndListener, replica endpoint = {\"Endpoints\":{\"\":\"Https:\/\/localhost:8491\/LocationApp\/\"}} ",
      "ProcessId": 57696,
      "Level": "Warning",
      "EventName": "ReverseProxy",
      "Payload": {
        "traceId": "c1cca3b7-f85d-4fef-a162-88af23604343",
        "requestUrl": "https://localhost:19081/LocationApp/LocationFEService?ListenerName=NewListener&zipcode=98052",
        ...
        "processRequestPhase": "TryGetEndoint",
        "errorDetails": "SecureOnlyMode = false, gateway protocol = https, listenerName = FrontEndListener, replica endpoint = {\"Endpoints\":{\"OldListener\":\"Https:\/\/localhost:8491\/LocationApp\/\"}}"
      }
    }
    ```
    Een ander voorbeeld waarbij 404 kan worden geretourneerd door reverse proxy is niet gevonden: configuratieparameter ApplicationGateway\Http **SecureOnlyMode** is ingesteld op waar met de reverse proxy luistert op **HTTPS**echter alle van de replica-eindpunten zijn niet-beveiligde (luistert naar HTTP).
    Reverse proxy retourneert 404 omdat er een eindpunt luistert naar HTTPS voor het doorsturen van de aanvraag kan niet worden gevonden. Analyse van de parameters in de gebeurtenis nettolading helpt om het probleem vast te stellen:
    
    ```
        "errorDetails": "SecureOnlyMode = true, gateway protocol = https, listenerName = NewListener, replica endpoint = {\"Endpoints\":{\"OldListener\":\"Http:\/\/localhost:8491\/LocationApp\/\", \"NewListener\":\"Http:\/\/localhost:8492\/LocationApp\/\"}}"
    ```

3. Aanvraag voor de omgekeerde proxy is mislukt met een time-outfout. 
    De gebeurtenislogboeken bevatten een gebeurtenis met de ontvangen request details (hier niet weergegeven).
    De volgende gebeurtenis ziet u dat de service met statuscode 404 reageerde en omgekeerde proxy te zetten initieert. 

    ```
    {
      ...
      "Message": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e Request to service returned: status code = 404, status description = , Reresolving ",
      "Payload": {
        "traceId": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e",
        "statusCode": 404,
        "statusDescription": ""
      }
    }
    {
      ...
      "Message": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e Re-resolved service url = Https://localhost:8491/LocationApp/?zipcode=98052 ",
      "Payload": {
        "traceId": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e",
        "requestUrl": "Https://localhost:8491/LocationApp/?zipcode=98052"
      }
    }
    ```
    Wanneer alle gebeurtenissen verzamelen, ziet u een trein van gebeurtenissen met elke oplossen en voorwaarts poging.
    De laatste gebeurtenis in de reeks ziet u dat de aanvraagverwerking is mislukt met een time-out, samen met het aantal pogingen voor geslaagde oplossen.
    
    > [!NOTE]
    > Het verdient aanbeveling te houden van de uitgebreide kanaal gebeurtenisverzameling is standaard uitgeschakeld en inschakelen voor het oplossen van problemen op basis van de nodig.

    ```
    {
      ...
      "Message": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e Error while processing request: number of successful resolve attempts = 12, error = FABRIC_E_TIMEOUT, message = , phase = ResolveServicePartition,  ",
      "EventName": "ReverseProxy",
      ...
      "Payload": {
        "traceId": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e",
        "resolveCount": 12,
        "errorval": -2147017729,
        "errorMessage": "",
        "processRequestPhase": "ResolveServicePartition",
        "errorDetails": ""
      }
    }
    ```
    
    Als de verzameling is ingeschakeld voor alleen kritieke fout/gebeurtenissen, ziet u een gebeurtenis met informatie over de time-out en het aantal pogingen oplossen. 
    
    Services die u van plan bent te verzenden van een statuscode 404 terug naar de gebruiker toe te voegen voor de koptekst van een 'X ServiceFabric' in het antwoord. Nadat de kop is toegevoegd aan het antwoord, stuurt de statuscode in omgekeerde proxy door terug naar de client.  

4. Gevallen wanneer de client heeft verbinding met de aanvraag.

    Volgende gebeurtenis wordt vastgelegd als omgekeerde proxy is het antwoord op client doorsturen, maar de client de verbinding verbreekt:

    ```
    {
      ...
      "Message": "6e2571a3-14a8-4fc7-93bb-c202c23b50b8 Unable to send response to client: phase = SendResponseHeaders, error = -805306367, internal error = ERROR_SUCCESS ",
      "ProcessId": 57696,
      "Level": "Warning",
      ...
      "EventName": "ReverseProxy",
      "Payload": {
        "traceId": "6e2571a3-14a8-4fc7-93bb-c202c23b50b8",
        "sendResponsePhase": "SendResponseHeaders",
        "errorval": -805306367,
        "winHttpError": "ERROR_SUCCESS"
      }
    }
    ```
5. Omgekeerde Proxy retourneert 404 FABRIC_E_SERVICE_DOES_NOT_EXIST

    FABRIC_E_SERVICE_DOES_NOT_EXIST fout is geretourneerd als de URI-schema niet is opgegeven voor de service-eindpunt in het servicemanifest.

    ```
    <Endpoint Name="ServiceEndpointHttp" Port="80" Protocol="http" Type="Input"/>
    ```

    Geef het URI-schema in het manifest van het probleem op te lossen.
    ```
    <Endpoint Name="ServiceEndpointHttp" UriScheme="http" Port="80" Protocol="http" Type="Input"/>
    ```

> [!NOTE]
> Gebeurtenissen met betrekking tot de verwerking van de websocket-aanvraag worden momenteel niet geregistreerd. Dit wordt toegevoegd in de volgende release.

## <a name="next-steps"></a>Volgende stappen
* [Gebeurtenis-aggregatie en verzameling op basis van Windows Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md) voor het inschakelen van logboekverzameling in Azure-clusters.
* Zie voor meer informatie over het bekijken van Service Fabric-gebeurtenissen in Visual Studio [bewaken en onderzoeken van lokaal](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).
* Raadpleeg [configureren omgekeerde proxy voor het verbinding maken met veilige services](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample#configure-reverse-proxy-to-connect-to-secure-services) voor Azure Resource Manager voorbeelden van de sjabloon voor het configureren van secure reverse proxy gebruikt met het certificaat voor verschillende validatieopties voor.
* Lees [Service Fabric omgekeerde proxy](service-fabric-reverseproxy.md) voor meer informatie.
