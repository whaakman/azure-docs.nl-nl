---
title: Toepassings- en problemen met de servicebeschikbaarheid voor veelgestelde vragen over Microsoft Azure Cloud Services | Microsoft Docs
description: In dit artikel geeft een lijst van de veelgestelde vragen over de toepassing en servicebeschikbaarheid voor Microsoft Azure Cloud Services.
services: cloud-services
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 617a6f4873c49877cbef48a7df3f2ab2c625445e
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50412513"
---
# <a name="application-and-service-availability-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Toepassings- en problemen met de servicebeschikbaarheid voor Azure Cloud Services: veelgestelde vragen

Dit artikel bevat veelgestelde vragen over de toepassing en problemen met de servicebeschikbaarheid voor [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). U kunt ook raadpleegt u de [VM-grootte voor Cloud Services-pagina](cloud-services-sizes-specs.md) voor informatie over de grootte.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="my-role-got-recycled-was-there-any-update-rolled-out-for-my-cloud-service"></a>Mijn rol is gerecycled. Is er een update geïmplementeerd voor mijn cloudservice?
Ongeveer brengt één keer per maand, Microsoft een nieuwe Guest OS-versie voor Windows Azure PaaS-VM's. Het Gastbesturingssysteem is slechts één update in de pijplijn. Een release kan worden beïnvloed door vele andere factoren. Bovendien Azure wordt uitgevoerd op honderden of duizenden machines. Daarom is het onmogelijk is om te voorspellen van de exacte datum en tijd wanneer uw functies wordt opnieuw opgestart. We de Gast OS bijwerken RSS-Feed bijwerken met de meest recente informatie die we hebben, maar u moet overwegen dat de tijd die een geschatte waarde wordt gerapporteerd. We weten dat dit problematisch voor klanten is en bezig bent met een plan om te beperken of nauwkeurig keer opnieuw wordt opgestart.

Zie voor meer informatie over recente updates van het Gast-besturingssysteem, [Azure Guest OS releases en SDK compatibiliteitsmatrix](cloud-services-guestos-update-matrix.md).

Voor nuttige informatie over het opnieuw is opgestart en verwijzingen naar de technische details van de Gast en Host-OS-updates, Zie het MSDN-blogbericht [rol exemplaar wordt opnieuw opgestart vanwege de Besturingssysteemupgrades](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx).

## <a name="why-does-the-first-request-to-my-cloud-service-after-the-service-has-been-idle-for-some-time-take-longer-than-usual"></a>Waarom de eerste aanvraag in mijn cloud-service nadat de service voor enige tijd inactief is geweest duurt langer dan normaal?
Wanneer de Server de eerste aanvraag ontvangt, eerst de code gecompileerd en verwerkt vervolgens de aanvraag. Daarom is de eerste aanvraag duurt langer dan de andere. Standaard wordt de groep van toepassingen afgesloten in geval van inactiviteit van de gebruiker. De app-groep wordt ook recycle standaard elke 1,740 minuten (29 uur).

Internet Information Services (IIS)-toepassing van toepassingen periodiek gerecycled worden kunnen om te voorkomen dat instabiel statussen die tot de toepassing leiden kunnen vastloopt, loopt vast, of geheugen lekt.

De volgende documenten kunt u te begrijpen en oplossen van dit probleem:
* [Trage laden oplossen voor IIS](http://stackoverflow.com/questions/13386471/fixing-slow-initial-load-for-iis)
* [IIS 7.5 web application eerste aanvraag na het recyclen van app-pool erg traag](http://stackoverflow.com/questions/13917205/iis-7-5-web-application-first-request-after-app-pool-recycle-very-slow)

Als u wijzigen van het standaardgedrag van IIS wilt, moet u gebruik opstarttaken, omdat het als u handmatig wijzigingen op de Webrol-exemplaren toepast, de wijzigingen uiteindelijk verbroken worden.

Zie voor meer informatie, [over het configureren en uitvoeren van opstarttaken voor een cloudservice](cloud-services-startup-tasks.md).
