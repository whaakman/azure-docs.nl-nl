---
title: Azure Stack beleid onderhoud | Microsoft Docs
description: Meer informatie over de Azure-Stack onderhoud beleid en hoe u een geïntegreerde systeem in een ondersteunde status.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: caac3d2f-11cc-4ff2-82d6-52b58fee4c39
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2018
ms.author: mabrigg
ms.openlocfilehash: e37b63580d8cea4b5772bc54f7b2f79980afc0bc
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="azure-stack-servicing-policy"></a>Azure Stack onderhoud van beleid
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
Om door te gaan ondersteuning krijgen, moet u de implementatie van uw Azure-Stack huidige bewaren. Het beleid voor uitstel van updates is voor Azure-Stack blijven worden ondersteund, moet het uitvoeren van de meest recent uitgebrachte versie van de update of voer een van de twee vorige versies van belangrijke update.  Hotfixes worden niet beschouwd als primaire updateversies.  Als uw Azure-Stack-cloud op achter is *meer dan twee updates*, dit wordt beschouwd als niet-compatibel en moeten bijwerken naar ten minste de minimaal ondersteunde versie om ondersteuning te ontvangen. 

Bijvoorbeeld, als de meest recente beschikbare updateversie 1805 is en de vorige twee updatepakketten versies 1804 en 1803 zijn, zowel 1803 1804 blijven en ondersteuning. 1802 is echter niet worden ondersteund. Het beleid geldt wanneer er geen release voor een maand of twee. Als de huidige release 1805 is en er geen 1804 release is, wordt door de vorige twee updatepakketten van 1803 en 1802 blijven in de ondersteuning.

Microsoft-software-updatepakketten zijn niet cumulatief en moeten het vorige updatepakket als een vereiste. Als u besluit om een of meer updates te stellen, moet u overwegen de algehele runtime als u wilt ophalen naar de nieuwste versie. 


## <a name="next-steps"></a>Volgende stappen

- [Beheren van updates in de Azure-Stack](azure-stack-updates.md)


