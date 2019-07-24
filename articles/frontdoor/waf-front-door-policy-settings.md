---
title: Beleids instellingen voor Web Application Firewall met Azure front-deur
description: Meer informatie Web Application Firewall (WAF).
services: frontdoor
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/08/2019
ms.author: kumud
ms.reviewer: tyao
ms.openlocfilehash: 8f51cb6944221416b098a9b953db417053155f1e
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2019
ms.locfileid: "67849111"
---
# <a name="policy-settings-for-web-application-firewall-with-azure-front-door"></a>Beleids instellingen voor Web Application Firewall met Azure front-deur

Met een WAF-beleid (Web Application firewall) kunt u de toegang tot uw webtoepassingen beheren met een set aangepaste en beheerde regels. De naam van het WAF-beleid moet uniek zijn. U krijgt een validatie fout als u een bestaande naam probeert te gebruiken. Er zijn meerdere beleids instellingen die van toepassing zijn op alle regels die voor dat beleid zijn opgegeven, zoals beschreven in dit artikel.

## <a name="waf-state"></a>WAF-status

Een WAF-beleid voor de voor deur kan een van de volgende twee statussen hebben:
- **Ingeschakeld** Wanneer een beleid is ingeschakeld, inspecteert WAF actief inkomende aanvragen en worden de bijbehorende acties uitgevoerd volgens regel definities
- **Uitgeschakeld:** -wanneer een beleid is uitgeschakeld, wordt de WAF-inspectie onderbroken. Binnenkomende aanvragen passeren WAF en worden naar back-ends verzonden op basis van front-deur routering.

## <a name="waf-mode"></a>WAF-modus

Het WAF-beleid kan worden geconfigureerd om te worden uitgevoerd in de volgende twee modi:

- **Detectie modus** Wanneer u de detectie modus uitvoert, neemt WAF geen andere acties dan de controle en registreert de aanvraag en de overeenkomende WAF-regel naar WAF-Logboeken. Diagnostische gegevens over logboek registratie inschakelen voor de voor deur (wanneer u Portal gebruikt, kunt u dit bereiken door naar de sectie **Diagnostische gegevens** in de Azure portal te gaan).

- **Preventie modus** Wanneer de WAF is geconfigureerd om te worden uitgevoerd in de preventie modus, neemt de opgegeven actie af als een aanvraag overeenkomt met een regel. Alle overeenkomende aanvragen worden ook in de WAF-logboeken vastgelegd.

## <a name="waf-response-for-blocked-requests"></a>WAF-antwoord voor geblokkeerde aanvragen

Wanneer WAF een aanvraag blokkeert vanwege een overeenkomende regel, retourneert deze standaard de status code 403 met- **de aanvraag is een geblokkeerd** bericht. Er wordt ook een verwijzings reeks geretourneerd voor logboek registratie.

U kunt een aangepaste antwoord status code en een antwoord bericht definiëren wanneer een aanvraag wordt geblokkeerd door WAF. De volgende aangepaste status codes worden ondersteund:

- 200 OK
- 403 verboden
- methode 405 niet toegestaan
- 406 niet toegestaan
- 429 te veel aanvragen

De status code en het antwoord bericht van de aangepaste reactie is een instelling voor het beleids niveau. Zodra deze is geconfigureerd, krijgen alle geblokkeerde aanvragen dezelfde aangepaste reactie status en een antwoord bericht.

## <a name="uri-for-redirect-action"></a>URI voor de omleidings actie

U moet een URI definiëren om aanvragen om te leiden als de omleidings actie is geselecteerd voor een van de regels die in een WAF-beleid zijn opgenomen. Deze omleidings-URI moet een geldige HTTP (S)-site zijn en eenmaal zijn geconfigureerd, worden alle aanvragen die overeenkomen met een actie ' omleiden ' omgeleid naar de opgegeven site.


## <a name="next-steps"></a>Volgende stappen
- Meer informatie over het definiëren van [aangepaste antwoorden](waf-front-door-configure-custom-response-code.md) op WAF
