---
title: Azure Stack beleid onderhoud | Microsoft Docs
description: "Meer informatie over de Azure-Stack onderhoud beleid en hoe u een geïntegreerde systeem in een ondersteunde status."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: caac3d2f-11cc-4ff2-82d6-52b58fee4c39
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: mabrigg
ms.openlocfilehash: 498d0cdc3eac25b8efc7339e48381a4d167c0c7b
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-stack-servicing-policy"></a>Azure Stack onderhoud van beleid

*Van toepassing op: Azure Stack geïntegreerd systemen*

Dit artikel wordt beschreven voor het beleid voor Azure-Stack geïntegreerd systemen, en wat u moet doen om uw systeem bewaren in een ondersteunde status voor onderhoud. 

## <a name="update-package-types"></a>Pakkettypen bijwerken

Er zijn twee typen van updatepakketten voor geïntegreerde systemen; Microsoft-software-updates en updates die specifiek zijn voor uw oorspronkelijke leveranciers (OEM) hardwareleverancier, zoals stuurprogramma's en firmware. Deze updates worden geleverd als afzonderlijke Azure Stack-updatepakketten en onafhankelijk van elkaar worden beheerd.

- **Microsoft software-updates**. Microsoft is verantwoordelijk voor het onderhoud levenscyclus van de end-to-end voor de Microsoft-software-updatepakketten. Deze pakketten kunnen zijn de meest recente beveiligingsupdates van Windows Server, niet-beveiligingsupdates en Azure Stack functie-updates. Bij deze updatepakketten kunt u rechtstreeks van Microsoft downloaden.
- **Door OEM geleverde leverancier updates van de hardware**. Azure Stack hardwarepartners zijn verantwoordelijk voor de end-to-end onderhoud lifecycle (inclusief richtlijnen) voor de hardware-gerelateerde firmware en stuurprogramma updatepakketten. Bovendien Azure Stack hardwarepartners eigenaar en onderhouden van de richtlijnen voor alle software en hardware op de host van de levenscyclus van hardware. De OEM-hardwareleverancier fungeert als host voor deze pakketten op hun eigen downloadsite bijwerken.

## <a name="update-package-release-cadence"></a>Update-pakket versie uitgebracht

Microsoft verwacht voor het vrijgeven van software-updatepakketten een maandelijkse uitgebracht. Het is echter mogelijk om meerdere of geen updates in een maand. OEM-hardwareleveranciers vrijgeven hun updates op een als dat nodig is.

Een Microsoft update-pakket heeft de volgende naamconventie kunt u gemakkelijk herkennen de releasedatum:

*MajorProductVersion.MinorProductVersion.YYMMDD.BuildNumber*

Een Microsoft-software-update uitgebracht op 15 juni 2017 zou bijvoorbeeld, de versie '1.0.170615.1' hebben.

## <a name="keep-your-system-under-support"></a>Houd uw systeem ondersteuning

Om ondersteuning te ontvangen voor uw systeem, moet u uw Azure-Stack bijgewerkt binnen een bepaald tijdsinterval. Onze beleid voor uitstel van Microsoft software-updates bestaat uit drie maanden. Als uw systeem meer dan drie maanden verouderd is, bent u niet-compatibel beschouwd. U moet het systeem bijwerken naar ten minste de minimaal ondersteunde versie voor het ontvangen van ondersteuning. 

Microsoft-software-updatepakketten zijn niet cumulatief en moeten het vorige updatepakket als een vereiste. Als u besluit om een of meer updates te stellen, moet u overwegen de algehele runtime als u wilt ophalen naar de nieuwste versie.

De volgende tabel bevat een voorbeeld van de update-pakket releases, hun vereisten en de minimaal ondersteunde versie die het systeem moet op voor verdere ondersteuning. De tabel is gebaseerd op de eerste versie van Azure-Stack geïntegreerd systemen (build 1708), met de eerste update pakket release (1709) in September 2017. 

| Meest recente updatepakket (*voorbeeld*) | Vereiste | Minimaal ondersteunde versie |
| -- | -- | -- |
| 1709 | Build 1708 | N.v.t. |
| 1710 | 1709 | N.v.t. |
| 1711 | 1710 | N.v.t. |
| 1712 | 1711 | 1709 |
| 1801 | 1712 | 1710 |
| 1802 | 1801 | 1711 |
| 1803 | 1802 | 1712 |
| 1804 | 1803 | 1801 |
| | | 

## <a name="next-steps"></a>Volgende stappen

- [Beheren van updates in de Azure-Stack](azure-stack-updates.md)


