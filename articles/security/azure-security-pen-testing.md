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
ms.openlocfilehash: 8835c4534b6dab1e8dbfb3546257ae4bc3b9d7af
ms.sourcegitcommit: 89b5e63945d0c325c1bf9e70ba3d9be6888da681
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2019
ms.locfileid: "57588649"
---
# <a name="penetration-testing"></a>Indringingstests
Een van de voordelen van het gebruik van Azure voor het testen van toepassingen en -implementatie, is dat u snel omgevingen die zijn gemaakt. U hoeft te breken over de aanschaf, ophalen, en "gestapel en stapelen ' uw eigen on-premises hardware.

Dit is fantastisch uit, maar u nog steeds nodig om ervoor te zorgen dat u de beveiliging van uw normale uitvoeren vanwege de zorgvuldigheid. Een van de dingen die u wilt waarschijnlijk doen indringingstests is het testen van de toepassingen die u implementeert in Azure.

U al kent die Microsoft uitvoert [indringingstests van onze Azure-omgeving](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e). Dit helpt het station verbeteringen in Azure.

We penetratietest niet uw toepassing, maar we begrijpen dat u wilt en wilt testen op uw eigen toepassingen uit te voeren. Dat is een goed, omdat wanneer u de beveiliging van uw toepassingen verbeteren u helpen u de volledige Azure-ecosysteem beter te beveiligen.

Microsoft vereist niet langer voorafgaande goedkeuring voor het uitvoeren van een penetratietest bij Azure-resources vanaf 15 juni 2017 gebruikt. Klanten die willen formeel document toekomstige indringingstests engagements op basis van Microsoft Azure worden uitgevers aangemoedigd om in te vullen de [testen-melding van Azure Service Indringingstests formulier](https://portal.msrc.microsoft.com/en-us/engage/pentest). Dit proces heeft alleen betrekking op Microsoft Azure en niet van toepassing op een andere Microsoft-Cloudservice.

>[!IMPORTANT]
>Terwijl de hoogte te brengen Microsoft van activiteiten voor pentests niet langer vereist is klanten nog steeds moeten voldoen aan de [Microsoft Cloud Unified Indringingstests testen regels van Engagement](https://technet.microsoft.com/mt784683).

Standaard tests uit die u kunt uitvoeren, zijn onder andere:

* Testen op uw eindpunten om de [Open Web Application Security Project (OWASP) top 10 beveiligingsproblemen](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)
* [Fuzzy testen](https://cloudblogs.microsoft.com/microsoftsecure/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/) van uw eindpunten
* [Scannen van poorten](https://en.wikipedia.org/wiki/Port_scanner) van uw eindpunten

Een type van de test die u niet kunt uitvoeren is een soort [Denial of Service (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack) aanval. Dit omvat een DoS-aanval zelf te initiëren of het uitvoeren van de bijbehorende tests die mogelijk te bepalen, laten zien of simuleren van elk type DoS-aanval.

## <a name="next-steps"></a>Volgende stappen

- Als u graag zou willen formeel document een aanstaande indringingstests op uw toepassingen die worden gehost in Microsoft Azure, gaat u op naar de [Indringingstests testen regels van Engagement](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=2) en vul het formulier voor testen aanmelding.
