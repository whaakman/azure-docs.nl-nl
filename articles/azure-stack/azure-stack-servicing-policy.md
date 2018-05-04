---
title: Azure Stack beleid onderhoud | Microsoft Docs
description: Meer informatie over de Azure-Stack onderhoud beleid en hoe u een geïntegreerde systeem in een ondersteunde status.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: caac3d2f-11cc-4ff2-82d6-52b58fee4c39
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2018
ms.author: brenduns
ms.reviewer: harik
ms.openlocfilehash: 77cc2f80588a104880e8149daccc6debd1ec43bc
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
---
# <a name="azure-stack-servicing-policy"></a>Azure Stack onderhoud van beleid
Dit artikel wordt beschreven voor het beleid voor Azure-Stack geïntegreerd systemen, en wat u moet doen om uw systeem bewaren in een ondersteunde status voor onderhoud. 

## <a name="update-package-types"></a>Pakkettypen bijwerken

Er zijn twee soorten updatepakketten voor geïntegreerde systemen: 

- **Microsoft software-updates**. Microsoft is verantwoordelijk voor het onderhoud levenscyclus van de end-to-end voor de Microsoft-software-updatepakketten. Deze pakketten kunnen zijn de meest recente beveiligingsupdates van Windows Server, niet-beveiligingsupdates en Azure Stack functie-updates. Bij deze updatepakketten kunt u rechtstreeks van Microsoft downloaden.

- **Door OEM geleverde leverancier updates van de hardware**. Azure Stack hardwarepartners zijn verantwoordelijk voor de end-to-end onderhoud lifecycle (inclusief richtlijnen) voor de hardware-gerelateerde firmware en stuurprogramma updatepakketten. Bovendien Azure Stack hardwarepartners eigenaar en onderhouden van de richtlijnen voor alle software en hardware op de host van de levenscyclus van hardware. De OEM-hardwareleverancier fungeert als host voor deze pakketten op hun eigen downloadsite bijwerken.


## <a name="update-package-release-cadence"></a>Update-pakket versie uitgebracht
Microsoft verwacht voor het vrijgeven van software-updatepakketten een maandelijkse uitgebracht. Het is echter mogelijk om meerdere of geen updates in een maand. OEM-hardwareleveranciers vrijgeven hun updates op een als dat nodig is. 

Documentatie over het plannen en beheren van updates en het bepalen van uw huidige versie in vinden [beheren updates overzicht](azure-stack-updates.md). Voor informatie over een specifieke update, met inbegrip van hoe u dit downloaden, Zie de releaseopmerkingen voor die bijwerken: 
- [Azure-Stack 1803 update](azure-stack-update-1803.md)
- [Azure-Stack 1802 update](azure-stack-update-1802.md)
- [Azure-Stack 1712 update](azure-stack-update-1712.md)



## <a name="hotfixes"></a>Hotfixes
In sommige gevallen kan biedt Microsoft hotfixes voor Azure-Stack dat een specifiek probleem die vaak preventief of tijdgebonden-adres.  Elke hotfix is uitgebracht met een bijbehorende Microsoft Knowledge Base-artikel waarin wordt uitgelegd van het probleem, oorzaak en oplossing. 

Hotfixes worden gedownload en geïnstalleerd net als de gewone volledige updatepakketten voor Azure-Stack. In tegenstelling tot een volledige update de hotfixes kunt installeren in minuten. U kunt het beste dat Azure Stack Operators onderhoudsvensters instellen bij het installeren van hotfixes. Hotfixes bijwerken de versie van uw Azure-Stack-cloud, zodat u gemakkelijk bepalen kunt of de hotfix is toegepast. Er is een afzonderlijke hotfix beschikbaar voor elke versie van Azure-Stack wordt nog steeds ondersteund. Elke oplossing voor een specifieke iteratie is cumulatief en bevat de vorige updates voor die dezelfde versie. U kunt meer lezen over de toepasselijkheid van een specifieke hotfix in een oplossingen overeenkomt Knowledge Base-artikel.  


## <a name="keep-your-system-under-support"></a>Houd uw systeem ondersteuning
Om door te gaan ondersteuning krijgen, moet u de implementatie van uw Azure-Stack huidige bewaren. Het beleid uitgestelde voor updates: voor uw Azure-Stack-implementatie blijft in de ondersteuning, moet het uitvoeren van de meest recent uitgebrachte versie van de update of voer een van de twee voorgaande updateversies. Hotfixes worden niet beschouwd als primaire updateversies. Als uw Azure-Stack-cloud op achter is *meer dan twee updates*, dit wordt beschouwd als niet-compatibel en moeten bijwerken naar ten minste de minimaal ondersteunde versie om ondersteuning te ontvangen. 

Bijvoorbeeld, als de meest recente beschikbare updateversie 1805 is en de vorige twee updatepakketten versies 1804 en 1803 zijn, zowel 1803 1804 blijven en ondersteuning. 1802 is echter niet worden ondersteund. Het beleid geldt wanneer er geen release voor een maand of twee. Bijvoorbeeld, als de huidige release 1805 is en er geen 1804 release is, blijven de vorige twee updatepakketten van 1803 en 1802 worden ondersteund.

Microsoft-software-updatepakketten zijn niet cumulatief en moeten het vorige updatepakket als een vereiste. Als u besluit om een of meer updates te stellen, moet u overwegen de algehele runtime als u wilt ophalen naar de nieuwste versie. 

## <a name="get-support"></a>Ondersteuning krijgen
Azure Stack volgt hetzelfde proces als Azure support. Enterprise-klanten kunnen Volg de procedure beschreven in [het maken van een aanvraag voor de ondersteuning van Azure](/azure/azure-supportability/how-to-create-azure-support-request). Als u een klant van een Cloud Service Provider (CSP), neem dan contact op met uw CSP voor ondersteuning.  Zie voor meer informatie de [Veelgestelde vragen over Azure-ondersteuning](https://azure.microsoft.com/support/faq/). 


## <a name="next-steps"></a>Volgende stappen

- [Beheren van updates in de Azure-Stack](azure-stack-updates.md)


