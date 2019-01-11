---
title: Azure Stack servicebeleid | Microsoft Docs
description: Meer informatie over de Azure Stack-beleid en hoe u een geïntegreerd systeem in een ondersteunde status onderhoud.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: caac3d2f-11cc-4ff2-82d6-52b58fee4c39
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2019
ms.author: sethm
ms.reviewer: harik
ms.openlocfilehash: db33ef743e595fe90d91769bf271f762fccade6b
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2019
ms.locfileid: "54212478"
---
# <a name="azure-stack-servicing-policy"></a>Azure Stack servicebeleid

Dit artikel wordt beschreven voor het uitvoeren van onderhoud beleid voor geïntegreerde Azure Stack-systemen, en wat u moet doen om te voorkomen dat uw systeem in een ondersteunde status.

## <a name="download-update-packages-for-integrated-systems"></a>Updatepakketten downloaden voor geïntegreerde systemen

Microsoft brengt zowel volledige maandelijkse updatepakketten als hotfixes voor specifieke problemen te verhelpen.

Updatepakketten met maandelijkse worden gehost in een beveiligde Azure-eindpunt. U kunt ze handmatig downloaden de [Azure Stack Updates downloader hulpprogramma](https://aka.ms/azurestackupdatedownload). Als uw schaaleenheid is verbonden, de update automatisch wordt weergegeven in de portal aan als beheerder **Update beschikbaar**. Volledige, worden maandelijks-updatepakketten ook beschreven op elke versie. Voor meer informatie over elke release kunt u een release van de [Update-pakket worden uitgerold](#update-package-release-cadence) sectie van dit artikel.

Hotfix-updatepakketten worden gehost in hetzelfde beveiligde Azure-eindpunt. U kunt ze handmatig met behulp van de ingesloten koppelingen in elk van de respectieve hotfix-KB-artikelen; downloaden bijvoorbeeld, [Azure Stack-Hotfix 1.1809.12.114](https://support.microsoft.com/help/4481548/azure-stack-hotfix-1-1809-12-114). Net als bij de volledige, maandelijkse updatepakketten, Azure Stack-operators kunnen de .xml, .BIN-bestand en .exe-bestanden downloaden en importeren met behulp van de procedure in [toepassen van updates in Azure Stack](azure-stack-apply-updates.md). Azure Stack-operators met verbonden schaaleenheden ziet de hotfixes automatisch weergegeven in de beheerdersportal met het bericht **Update beschikbaar**.

Als uw schaaleenheid niet is verbonden en u wilt worden gewaarschuwd over elke versie van de hotfix, zich abonneren op de [RSS](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss … ) of [ATOM](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom …) feed die u hebt genoteerd in elke versie.  

## <a name="update-package-types"></a>Pakkettypen bijwerken

Er zijn twee soorten updatepakketten voor geïntegreerde systemen:

- **Microsoft-software-updates**. Microsoft is verantwoordelijk voor het uitvoeren van onderhoud levenscyclus van de end-to-end voor de Microsoft-software-updatepakketten. Deze pakketten kunnen opnemen de meest recente beveiligingsupdates van Windows Server, niet-beveiligingsupdates en functie-updates voor Azure Stack. U kunt deze updatepakketten downloaden rechtstreeks van Microsoft.

- **OEM hardware leverancier verstrekte updates**. Azure Stack-hardware-partners zijn verantwoordelijk voor de end-to-end servicing-levenscyclus (met inbegrip van richtlijnen) voor de hardware-gerelateerde firmware en stuurprogramma-updatepakketten. Bovendien Azure Stack-hardwarepartners eigenaar en onderhouden van de richtlijnen voor alle software en hardware op de host van de levenscyclus van hardware. De hardware van OEM-leverancier als host fungeert voor deze pakketten op hun eigen downloadsite bijwerken.

## <a name="update-package-release-cadence"></a>Vrijgeeffrequentie pakket bijwerken

Microsoft verwacht om software-updatepakketten een maandelijkse uitgebracht vrij te geven. Het is echter mogelijk om meerdere, of er zijn geen updates in een maand. OEM-hardwareleveranciers vrijgeven hun updates op basis van behoefte.

Documentatie voor het plannen en beheren van updates en het bepalen van uw huidige versie in [beheren updates overzicht](azure-stack-updates.md).

Voor informatie over een bepaalde update, met inbegrip van hoe u het downloaden, Zie de releaseopmerkingen voor deze update:

- [Azure Stack 1811 update](azure-stack-update-1811.md)
- [Azure Stack 1809 update](azure-stack-update-1809.md)
- [Azure Stack 1808 update](azure-stack-update-1808.md)
- [Azure Stack 1807 update](azure-stack-update-1807.md)

## <a name="hotfixes"></a>Hotfixes

In sommige gevallen biedt Microsoft hotfixes voor Azure Stack dat adres een specifiek probleem die vaak preventief of tijdgebonden.  Elke hotfix is uitgebracht met een bijbehorende Microsoft Knowledge Base-artikel waarin wordt uitgelegd van het probleem, de oorzaak en de oplossing.

Hotfixes worden gedownload en geïnstalleerd net als bij de reguliere volledige update-pakketten voor Azure Stack. In tegenstelling tot een volledige update de hotfixes kunt installeren in minuten. U wordt aangeraden dat Azure Stack-Operators Stel de onderhoudsvensters bij het installeren van hotfixes. Hotfixes werk de versie van uw Azure Stack-cloud, zodat u eenvoudig bepalen kunt of de hotfix is toegepast. Er is een afzonderlijke hotfix beschikbaar voor elke versie van Azure-Stack die wordt nog steeds ondersteund. Elke oplossing voor een specifieke herhaling is cumulatief en bevat de vorige updates voor die dezelfde versie. U kunt meer lezen over de toepasselijkheid van een specifieke hotfix in een correcties bijbehorende Knowledge Base-artikel.  

## <a name="keep-your-system-under-support"></a>Houd uw systeem onder ondersteuning

Als u wilt doorgaan om ondersteuning te ontvangen, moet u uw Azure Stack-implementatie actueel houden. Het beleid om op updates is: Voor uw Azure Stack-implementatie moet worden bewaard in de ondersteuning, moet het uitvoeren van de meest recent uitgebrachte versie van de update of een van de twee voorgaande updateversies van de uitvoeren. Hotfixes worden niet beschouwd als grote update-versies. Als uw Azure Stack-cloud op is achter *meer dan twee updates*, deze wordt beschouwd als niet-compatibel en moeten bijwerken naar ten minste de minimaal ondersteunde versie om ondersteuning te ontvangen.

Bijvoorbeeld, als de meest recente beschikbare versie van de update 1805 is en de vorige twee updatepakketten versies 1804 en 1803 zijn, blijven zowel 1803 1804 en ondersteuning. 1802 is echter ondersteuning vervalt. Het beleid geldt wanneer er geen versie voor een maand of twee. Bijvoorbeeld, als de huidige versie 1805 is en er geen 1804 release is, blijven de vorige twee updatepakketten 1803 en 1802 ondersteund.

Microsoft-software-updatepakketten zijn niet cumulatief en vereisen dat de vorige update of hotfix als een vereiste. Als u besluit om een of meer updates uit te stellen, kunt u overwegen de totale runtime als u wilt ontvangen naar de nieuwste versie.

## <a name="get-support"></a>Ondersteuning krijgen

Azure Stack volgt hetzelfde ondersteuningsproces als Azure. Enterprise-klanten kunnen gaat u als volgt de procedure beschreven in [over het maken van een Azure-ondersteuningsaanvraag](/azure/azure-supportability/how-to-create-azure-support-request). Als u een klant bent van een Cloud Service Provider (CSP), neem dan contact op met uw CSP voor ondersteuning.  Zie voor meer informatie de [Veelgestelde vragen over Azure-ondersteuning](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Volgende stappen

- [Updates beheren in Azure Stack](azure-stack-updates.md)