---
title: Pen testen | Microsoft Docs
description: Het artikel biedt een overzicht van het binnendringen testproces (pentest) en hoe uitvoeren pentest vergelijken met uw apps uitgevoerd in de Azure-infrastructuur.
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: 695d918c-a9ac-4eba-8692-af4526734ccc
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: yurid
ms.openlocfilehash: 3ad22e78693c54c62f9230f7f52460e01e5e0022
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/23/2017
---
# <a name="pen-testing"></a>Pen testen
Een van de voordelen van het gebruik van Azure voor de implementatie en het testen van toepassingen is dat u snel omgevingen gemaakt kunt krijgen.  U hoeft te hoeven maken over aanschaf, ophalen, en ' bekabeld en stapelen ' uw eigen lokale hardware.

Dit is fantastisch uit, maar u toch wilt controleren of u de beveiliging van uw normale uitvoeren innen. Een van de dingen die u moet doen binnendringen is het testen van de toepassingen die u implementeert in Azure.

Kent u mogelijk al die Microsoft uitvoert [binnendringen testen van ons Azure-omgeving](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e). Dit helpt station verbeteringen in Azure. 

We niet pen testen van uw toepassing, maar we begrijpen dat u wilt en moet pen testen op uw eigen toepassingen uitvoeren. Dat is goed, omdat het als u de beveiliging van uw toepassingen verbeteren, u helpen bij het beter beveiligen van de volledige Azure-ecosysteem.

Wanneer u pen test uw toepassingen, kan deze een aanval eruitzien aan ons. We [continu bewaken](http://blogs.msdn.com/b/azuresecurity/archive/2015/07/05/best-practices-to-protect-your-azure-deployment-against-cloud-drive-by-attacks.aspx) voor aanvalspatronen en initieert een respons op incidenten proces als worden moet. Dit niet helpt u en deze niet ons als we een respons op incidenten als gevolg van uw eigen vanwege toewijding inzetten pen tests activeert.

Hoe pakt u dit aan?

Wanneer u klaar bent om pen uw Azure gehoste toepassingen te testen, hebt u een optie voor het [laat ons weten](https://portal.msrc.microsoft.com/en-us/engage/pentest). Kennisgeving van wordt Microsoft niet per ongeluk sluit u (zoals het IP-adres dat u test worden geblokkeerd). Uw tests moeten voldoen aan de Azure pen testen voorwaarden en voorwaarden die worden beschreven [Microsoft Cloud Unified binnendringen testen regels van Engagement](https://technet.microsoft.com/en-us/mt784683).

Standaard tests uit die u kunt uitvoeren, zijn onder andere:

* Tests op uw eindpunten om leeg te onthullen de [Open Web Application Security Project (OWASP) top 10 beveiligingsproblemen](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)
* [Fuzzy testen](https://blogs.microsoft.com/cybertrust/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/) van uw eindpunten
* [Scannen van poorten](https://en.wikipedia.org/wiki/Port_scanner) van uw eindpunten

Een type van test die u niet uitvoeren, is een soort [Denial of Service (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack) aanval. Dit omvat een DoS-aanval zelf te initiëren of het uitvoeren van de bijbehorende tests die mogelijk bepalen, laten zien of simuleren van elk type DoS-aanval.

## <a name="next-steps"></a>Volgende stappen

- Test u gereed om te beginnen met pen uw toepassingen die worden gehost in Microsoft Azure? Als er in dat geval, head op boven aan de [binnendringen Test overzicht](https://technet.microsoft.com/library/mt784683.aspx) pagina (en klik op de knop testen aanvragen aan de onderkant van de pagina maken. 