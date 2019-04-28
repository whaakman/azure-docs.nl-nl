---
title: Beleidsinstellingen voor de web application firewall voordeur met Azure
description: Meer informatie over de web application firewall (WAF).
services: frontdoor
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/08/2019
ms.author: tyao;kumud
ms.openlocfilehash: 4c2f070e9b3c972f063008df8880b196ddb069cc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "61459365"
---
# <a name="policy-settings-for-web-application-firewall-with-azure-front-door"></a>Beleidsinstellingen voor de web application firewall voordeur met Azure

Een Web Application Firewall (WAF)-beleid kunt u toegang tot uw webtoepassingen beheren door een set aangepaste en beheerde regels. De naam van de WAF-beleid moet uniek zijn. U ontvangt een validatiefout als u probeert te gebruiken van een bestaande naam. Er zijn meerdere beleid niveau instellingen die betrekking hebben op alle regels die zijn opgegeven voor dit beleid, zoals beschreven in dit artikel.

## <a name="waf-state"></a>WAF-status

Een WAF-beleid voor voordeur kan zich in een van de volgende twee statussen:
- **Ingeschakeld:** Wanneer een beleid is ingeschakeld, WAF is actief inspecteren van binnenkomende aanvragen en bijbehorende acties op basis van regeldefinities
- **Uitgeschakeld:** : wanneer een beleid is uitgeschakeld, controle van de WAF wordt onderbroken. Inkomende aanvragen WAF wordt omzeilen en worden verzonden naar de back-ends op basis van de voordeur routering.

## <a name="waf-mode"></a>WAF-modus

WAF-beleid kan worden geconfigureerd om te worden uitgevoerd in de volgende twee modi:

- **Detectiemodus** wanneer uitvoert in de modus voor detectie, WAF niet nemen maatregelen dan monitor en meld u de aanvraag en de overeenkomende WAF-regel voor WAF-Logboeken. Diagnostische logboekregistratie inschakelen voor de voordeur (wanneer u portal gebruikt, kunt u dit doen door te gaan naar de **Diagnostics** sectie in Azure portal).

- **Preventiemodus** wanneer geconfigureerd om te worden uitgevoerd in preventiemodus, WAF de opgegeven actie onderneemt als een aanvraag overeenkomt met een regel. Geen overeenkomende aanvragen worden ook geregistreerd in de WAF-Logboeken.

## <a name="waf-response-for-blocked-requests"></a>WAF-reactie voor geblokkeerde aanvragen

Standaard, wanneer een aanvraag wordt door de WAF geblokkeerd vanwege een overeenkomende regel wordt een 403-statuscode met - **de aanvraag is geblokkeerd** bericht. Een verwijzingstekenreeks wordt ook geretourneerd als voor aanmelding.

U kunt een aangepaste antwoordstatuscode en een antwoordbericht definiëren wanneer een aanvraag wordt geblokkeerd door WAF. De volgende aangepaste statuscodes worden ondersteund:

- 200    OK
- 403    Forbidden
- 405 methode is niet toegestaan
- 406 niet acceptabel
- 429 te veel aanvragen

Aangepaste status en de reactietijd antwoordbericht is een niveau beleidsinstelling. Nadat deze is geconfigureerd, krijgen alle geblokkeerde aanvragen de dezelfde aangepast antwoordstatus en het antwoordbericht.

## <a name="uri-for-redirect-action"></a>URI voor omleiden actie

U bent verplicht om te definiëren van een URI voor het omleiden van aanvragen om te genereren als de **OMLEIDEN** actie voor een van de regels in een WAF-beleid is ingeschakeld. Deze omleidings-URI moet een geldige HTTP (S)-site en wanneer dit is geconfigureerd, alle aanvragen overeenkomende regels met een "OMLEIDING"-actie wordt omgeleid naar de opgegeven site.


## <a name="next-steps"></a>Volgende stappen
- Meer informatie over het definiëren van WAF [aangepaste antwoorden](waf-front-door-configure-custom-response-code.md)
