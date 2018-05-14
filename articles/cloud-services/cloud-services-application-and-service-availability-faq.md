---
title: Toepassings- en problemen met de servicebeschikbaarheid voor veelgestelde vragen over Microsoft Azure Cloud Services | Microsoft Docs
description: Dit artikel worden de veelgestelde vragen over de toepassing en servicebeschikbaarheid voor Microsoft Azure Cloud Services.
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
ms.date: 05/11/2018
ms.author: genli
ms.openlocfilehash: 49576aa99f6cd505648e33348b89e502bea9d5c4
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
---
# <a name="application-and-service-availability-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Toepassings- en problemen met de servicebeschikbaarheid voor Azure Cloud Services: veelgestelde vragen (FAQ's)

Dit artikel bevat veelgestelde vragen over de toepassing en problemen met de servicebeschikbaarheid voor [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). U kunt ook raadpleegt u de [VM-grootte voor Cloud Services-pagina](cloud-services-sizes-specs.md) voor informatie over de grootte.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="my-role-got-recycled-was-there-any-update-rolled-out-for-my-cloud-service"></a>Mijn rol is gerecycled. Is er een update voor mijn cloudservice uitgerold?
Min of meer versies maand, Microsoft een nieuwe versie van de Gast OS voor PaaS VM's van Windows Azure. Het Gastbesturingssysteem is slechts één update in de pijplijn. Een release kan worden beïnvloed door veel andere factoren. Bovendien Azure wordt uitgevoerd op honderden of duizenden computers. Daarom is het onmogelijk om het voorspellen van de exacte datum en tijd waarop de rollen opnieuw wordt opgestart. We de Gast OS bijwerken van RSS-Feed bijwerken met de meest recente informatie die we hebben, maar u moet rekening houden met die tijd die een geschatte waarde wordt gerapporteerd. We zijn dit is problematisch voor klanten en werkt aan een plan om te beperken of nauwkeurig keer opnieuw wordt opgestart.

Zie voor meer informatie over recente updates voor gast OS [Azure Gast OS releases en SDK compatibiliteit matrix](cloud-services-guestos-update-matrix.md).

Zie het blogbericht MSDN voor nuttige informatie over het opnieuw is opgestart en koppelingen naar technische details van de updates voor gast en Host-OS [rol exemplaar opnieuw wordt opgestart vanwege Upgrades voor het besturingssysteem](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx).

## <a name="why-does-the-first-request-to-my-cloud-service-after-the-service-has-been-idle-for-some-time-take-longer-than-usual"></a>Waarom het eerste verzoek bij de service in de cloud nadat de service niet actief is geweest gedurende een bepaalde periode duurt langer dan normaal?
Wanneer de webserver de eerste aanvraag ontvangt, gecompileerd eerst de code en de aanvraag wordt verwerkt. Daarom is de eerste aanvraag duurt langer dan de andere. Standaard wordt de groep van toepassingen afgesloten in geval van een gebruiker inactiviteit. De groep van toepassingen wordt ook recycle standaard elke 1,740 minuten (29 uur).

Internet Information Services (IIS)-toepassing groepen regelmatig opnieuw worden gebruikt worden kunnen om te voorkomen dat onstabiel statussen die tot de toepassing leiden kunnen is vastgelopen, loopt vast, of geheugen lekt.

De volgende documenten helpt u begrijpen en dit probleem te verhelpen:
* [Trage initiële laden is hersteld voor IIS](http://stackoverflow.com/questions/13386471/fixing-slow-initial-load-for-iis)
* [IIS 7.5 toepassing eerste webaanvraag na-toepassingen recyclen erg traag](http://stackoverflow.com/questions/13917205/iis-7-5-web-application-first-request-after-app-pool-recycle-very-slow)

Als u wijzigen van het standaardgedrag van IIS wilt, moet u u taken starten, omdat het als u handmatig wijzigingen op de exemplaren van de Webrol toepast, de wijzigingen uiteindelijk worden gewist.

Zie voor meer informatie [configureren en starten van de taken voor een cloudservice uitvoeren](cloud-services-startup-tasks.md).
