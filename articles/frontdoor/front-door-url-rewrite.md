---
title: Azure Front deur Service - URL herschrijven | Microsoft Docs
description: Dit artikel helpt u begrijpen hoe Azure voordeur Service doet herschrijven van URL's voor uw routes als geconfigureerd.
services: front-door
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 00fe3aa7a641b9d07aad90a9d008a99efc6e9d97
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46993462"
---
# <a name="url-rewrite-custom-forwarding-path"></a>URL opnieuw schrijven (aangepaste doorsturen via het pad)
Azure voordeur-Service ondersteunt herschrijven van URL doordat u een optionele configureren **aangepast doorsturen pad** te gebruiken bij het maken van de aanvraag door te sturen naar de back-end. Standaard als geen pad aangepaste doorsturen is opgegeven, kopieert klikt u vervolgens voordeur het binnenkomende URL-pad naar de URL die wordt gebruikt in de aanvraag doorgestuurd. De Host-header wordt gebruikt in de aanvraag doorgestuurd is geconfigureerd voor de geselecteerde back-end. Lezen [Host-Header van back-end](front-door-backend-pool.md#hostheader) voor meer informatie over wat het doet en hoe u deze kunt configureren.

De krachtig onderdeel van het herschrijven van URL met behulp van aangepaste doorsturen pad is dat deze een deel van het binnenkomende pad die overeenkomt met een jokerteken pad naar de doorgestuurde pad worden gekopieerd (deze padsegmenten zijn de **groen** segmenten in het onderstaande voorbeeld):
</br>
![Azure voordeur URL opnieuw schrijven][1]

## <a name="url-rewrite-example"></a>Voorbeeld van de URL opnieuw schrijven
Houd rekening met een regel voor doorsturen met de volgende frontend-hosts en de paden die zijn geconfigureerd:

| Hosts      | Paden       |
|------------|-------------|
| www.contoso.com | /\*         |
|            | /foo        |
|            | /foo/\*     |
|            | /foo/balk /\* |

De eerste kolom van de onderstaande tabel ziet u voorbeelden van inkomende aanvragen en de tweede kolom geeft aan wat het 'meest-specific' overeenkomende route 'Path' zou zijn.  De derde en de volgende kolommen van de eerste rij van de tabel zijn voorbeelden van geconfigureerde **aangepaste doorsturen paden**, met de rest van de rijen in die kolommen voor voorbeelden van wat de doorgestuurde Aanvraagpad zijn zou als het resultaat gevonden aan de aanvraag in die rij.

Bijvoorbeeld, als we in de tweede rij lezen, wordt ontdekt dat die voor de inkomende aanvraag `www.contoso.com/sub`, als het pad van de aangepaste doorsturen is `/`, bedragen de doorgestuurde pad `/sub`. Als het pad van de aangepaste doorsturen is `/fwd/`, bedragen de doorgestuurde pad `/fwd/sub`. Enzovoort, voor de overige kolommen. De **FDA** onderdelen van de paden die hieronder staan de gedeelten die deel van het jokerteken uitmaken.


| Inkomende aanvraag       | Pad naar de meest specifieke overeenkomst | /          | /FWD/          | /foo/          | /foo/balk /          |
|------------------------|--------------------------|------------|----------------|----------------|--------------------|
| www.contoso.com/            | /\*                      | /          | /FWD/          | /foo/          | /foo/balk /          |
| www.contoso.com/**sub**     | /\*                      | /**Sub**   | /FWD/**sub**   | /foo/**sub**   | /foo/balk/**sub**   |
| www.contoso.com/**a/b/c**   | /\*                      | /**a/b/c** | /FWD/**a/b/c** | /foo/**a/b/c** | /foo/balk/**a/b/c** |
| www.contoso.com/foo         | /foo                     | /          | /FWD/          | /foo/          | /foo/balk /          |
| www.contoso.com/foo/        | /foo/\*                  | /          | /FWD/          | /foo/          | /foo/balk /          |
| www.contoso.com/foo/**balk** | /foo/\*                  | /**balk**   | /FWD/**balk**   | /foo/**balk**   | /foo/balk/**balk**   |


## <a name="optional-settings"></a>Optionele instellingen
Er zijn aanvullende optionele instellingen die u ook voor een bepaalde routering regelinstellingen opgeven kunt:

* **Configuratie van de cache** - als uitgeschakeld of niet is opgegeven, en vervolgens de aanvragen die met deze regel voor doorsturen overeenkomen niet probeert te gebruiken in de cache inhoud en in plaats daarvan worden altijd opgehaald uit de back-end. Meer informatie over [Caching met voordeur](front-door-caching.md).



## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [maken van een voordeur](quickstart-create-front-door.md).
- Informatie over [de werking van de voordeur](front-door-routing-architecture.md).

<!--Image references-->
[1]: ./media/front-door-url-rewrite/front-door-url-rewrite-example.jpg