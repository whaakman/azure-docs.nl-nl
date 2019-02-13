---
title: Pen testen | Microsoft Docs
description: Het artikel bevat een overzicht van de indringingstests (pentest)-proces en hoe uitvoeren pentest op basis van uw toepassingen die worden uitgevoerd in Azure-infrastructuur.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 695d918c-a9ac-4eba-8692-af4526734ccc
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/13/2018
ms.author: barclayn
ms.openlocfilehash: bfe289e29a6a2bb048d9fd42323f32ce8c12792c
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56110605"
---
# <a name="pen-testing"></a>Pen testen
Een van de voordelen van het gebruik van Azure voor het testen van toepassingen en -implementatie, is dat u snel omgevingen die zijn gemaakt.  U hoeft te breken over de aanschaf, ophalen, en "gestapel en stapelen ' uw eigen on-premises hardware.

Dit is fantastisch uit, maar u nog steeds nodig om ervoor te zorgen dat u de beveiliging van uw normale uitvoeren vanwege de zorgvuldigheid. Een van de dingen die u moet doen is indringingstests testen van de toepassingen die u implementeert in Azure.

U al kent die Microsoft uitvoert [indringingstests van onze Azure-omgeving](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e). Dit helpt het station verbeteringen in Azure.

We geen pen testen van uw toepassing, maar we begrijpen dat u wilt en hoeft uit te voeren pen testen op uw eigen toepassingen. Dat is een goed, omdat wanneer u de beveiliging van uw toepassingen verbeteren, u helpen u de volledige Azure-ecosysteem beter te beveiligen.

Hoe pakt u dit aan?

Vanaf 15 juni 2017, Microsoft vereist niet langer voorafgaande goedkeuring voor het uitvoeren van een indringing tests bij Azure-resources. Klanten die willen formeel document toekomstige indringingstests engagements op basis van Microsoft Azure worden uitgevers aangemoedigd om in te vullen de [testen-melding van Azure Service Indringingstests formulier](https://portal.msrc.microsoft.com/en-us/engage/pentest). Dit proces heeft alleen betrekking op Microsoft Azure en niet van toepassing op een andere Microsoft-Cloudservice.

>[!IMPORTANT]
>Terwijl de hoogte te brengen Microsoft van activiteiten voor pentests niet langer vereist is klanten nog steeds moeten voldoen aan de [Microsoft Cloud Unified Indringingstests testen regels van Engagement](https://technet.microsoft.com/mt784683).

Standaard tests uit die u kunt uitvoeren, zijn onder andere:

* Testen op uw eindpunten om de [Open Web Application Security Project (OWASP) top 10 beveiligingsproblemen](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)
* [Fuzzy testen](https://cloudblogs.microsoft.com/microsoftsecure/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/) van uw eindpunten
* [Scannen van poorten](https://en.wikipedia.org/wiki/Port_scanner) van uw eindpunten

Een type van de test die u niet kunt uitvoeren is een soort [Denial of Service (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack) aanval. Dit omvat een DoS-aanval zelf te initiëren of het uitvoeren van de bijbehorende tests die mogelijk te bepalen, laten zien of simuleren van elk type DoS-aanval.

## <a name="next-steps"></a>Volgende stappen

- U kunt aan de slag met pen Test uw toepassingen die worden gehost in Microsoft Azure Als dit het geval is, gaat u op naar de [Indringingstests testen regels van Engagement](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=2) en vul het formulier voor testen aanmelding.
