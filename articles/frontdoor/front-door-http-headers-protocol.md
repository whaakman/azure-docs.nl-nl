---
title: Voordeur-Service, Azure - ondersteuning voor HTTP-headers | Microsoft Docs
description: Dit artikel helpt u begrijpen van de ondersteunde protocollen van de HTTP-header met de voordeur
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 40bfdfc3837da12f62864433508482a65def291c
ms.sourcegitcommit: 280d9348b53b16e068cf8615a15b958fccad366a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58407314"
---
# <a name="azure-front-door-service---http-headers-support"></a>Voordeur-Service, Azure - ondersteuning voor HTTP-headers
Dit document bevat een overzicht van het protocol dat Azure voordeur Service met de verschillende onderdelen van het pad van de aanroep ondersteunt zoals wordt beschreven door de onderstaande afbeelding. De onderstaande secties vindt u meer inzicht in de HTTP-Headers die ondersteuning biedt voor voordeur.

![Azure voordeur Service HTTP-headers-protocol][1]

>[!WARNING]
>Azure voordeur Service biedt geen garanties op een HTTP-headers die hier niet wordt vermeld.

## <a name="1-client-to-front-door"></a>1. Client naar de voordeur
Voordeur accepteert de meeste headers van de inkomende aanvraag (zonder deze te wijzigen), er zijn echter enkele gereserveerde headers die wordt verwijderd uit de binnenkomende aanvraag als ze worden verzonden. Dit omvat headers met de volgende voorvoegsels:
 - X-FD-*

## <a name="2-front-door-to-backend"></a>2. Voordeur naar back-end

Voordeur bevat de headers van de inkomende aanvraag, tenzij ze zijn verwijderd vanwege de beperkingen die hierboven worden vermeld. Voordeur wordt ook de volgende headers toevoegen:

| Header  | Voorbeeld en beschrijving |
| ------------- | ------------- |
| Via |  *Via: 1.1 Azure* </br> Voordeur toegevoegd van de client HTTP-versie, gevolgd door 'Azure' als de waarde voor Via-header. Deze is toegevoegd om aan te geven van de client HTTP-versie en dat voordeur Azure is een tussenliggende ontvanger voor de aanvraag tussen de client en de back-end.  |
| X-Azure-ClientIP | *X-Azure-ClientIP: 127.0.0.1* </br> Hiermee geeft u de 'client' IP-adres dat is gekoppeld aan de aanvraag wordt verwerkt. Een aanvraag die afkomstig zijn van een proxy kan bijvoorbeeld de header X doorgestuurd voor om aan te geven van de IP-adres van de oorspronkelijke aanvrager toevoegen. |
| X-Azure-SocketIP | *X-Azure-SocketIP: 127.0.0.1* </br> Hiermee geeft u de socket-IP-adres dat is gekoppeld aan de TCP-verbinding, de huidige aanvraag afkomstig is uit. Client-IP-adres van een aanvraag zijn mogelijk niet gelijk zijn aan het Socket-IP-adres omdat deze willekeurig kan worden overschreven door een eindgebruiker.|
| X-Azure-Ref | *X-Azure-Ref: 0zxV+XAAAAABKMMOjBv2NT4TY6SQVjC0zV1NURURHRTA2MTkANDM3YzgyY2QtMzYwYS00YTU0LTk0YzMtNWZmNzA3NjQ3Nzgz* </br> Dit is een unieke referentie-tekenreeks die een aanvraag die is geleverd door de voordeur aanduidt. Het is essentieel voor het oplossen van problemen zoals het wordt gebruikt om te zoeken naar de logboeken openen.|
| X-Azure-RequestChain |  *X-Azure-RequestChain: hops voor de = 1* </br> Dit is een header die voordeur wordt gebruikt voor het detecteren van aanvraag lussen en een afhankelijkheid niet op deze gebruikers moeten uitvoeren. |
| X doorgestuurd voor | *X doorgestuurd voor: 127.0.0.1* </br> Het veld X-Forwarded-For (XFF) HTTP-header is een veelgebruikte methode voor het identificeren van het oorspronkelijke IP-adres van een client die verbinding maken met een webserver via een HTTP-proxy- of load balancer. Als er een bestaande XFF-header, is en vervolgens de voordeur het client-socket-IP-adres toegevoegd aan deze anders voegt u de XFF-header met de client-IP-socket. |
| X-doorgestuurd-Host | *X-doorgestuurd-Host: contoso.azurefd.net* </br> Het veld met X-doorgestuurd-Host HTTP-header is een veelgebruikte methode voor het identificeren van de oorspronkelijke host aangevraagd door de client in de aanvraagheader HTTP-Host, omdat de hostnaam van de voordeur kan verschillen voor de back-endserver verwerken van de aanvraag. |
| X-Forwarded-Proto | *X-doorgestuurd-protocol: http* </br> Het veld met X-doorgestuurd Protoco HTTP-header is een veelgebruikte methode voor het identificeren van het oorspronkelijke protocol van een HTTP-aanvraag, aangezien, afhankelijk van de configuratie van de voordeur met de back-end via HTTPS communiceren kan, zelfs als de aanvraag voor de omgekeerde proxy HTTP is. |

## <a name="3-front-door-to-client"></a>3. Voordeur naar client

Hieronder vindt u de headers die zijn verzonden vanaf de voordeur aan clients. Alle headers worden verzonden naar de voordeur vanaf de back-end worden doorgegeven aan de client ook.

| Header  | Voorbeeld |
| ------------- | ------------- |
| X-Azure-Ref |  *X-Azure-Ref: 0zxV+XAAAAABKMMOjBv2NT4TY6SQVjC0zV1NURURHRTA2MTkANDM3YzgyY2QtMzYwYS00YTU0LTk0YzMtNWZmNzA3NjQ3Nzgz* </br> Dit is een unieke referentie-tekenreeks die een aanvraag die is geleverd door de voordeur aanduidt. Het is essentieel voor het oplossen van problemen zoals het wordt gebruikt om te zoeken naar de logboeken openen.|

## <a name="next-steps"></a>Volgende stappen

- Lees hoe u [een Front Door maakt](quickstart-create-front-door.md).
- Lees [hoe Front Door werkt](front-door-routing-architecture.md).

<!--Image references-->
[1]: ./media/front-door-http-headers-protocol/front-door-protocol-summary.png