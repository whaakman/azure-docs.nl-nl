---
title: Ondersteuningsmogelijkheden en buiten gebruik stellen beleid handleiding voor Azure Gast OS | Microsoft Docs
description: Bevat informatie over wat wordt ondersteuning van Microsoft met betrekking tot aan de Azure-Gastbesturingssysteemreleases door de Cloud-Services gebruikt.
services: cloud-services
documentationcenter: na
author: raiye
manager: timlt
editor: 
ms.assetid: 919dd781-4dc6-4e50-bda8-9632966c5458
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 9/20/2017
ms.author: raiye
ms.openlocfilehash: 4bc2d57cf4c7d6e0981aa1a5c7c989860600b897
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-guest-os-supportability-and-retirement-policy"></a>Azure Gast OS ondersteuningsmogelijkheden en buiten gebruik stellen beleid
De informatie op deze pagina is gekoppeld aan het gastbesturingssysteem van Azure ([Gastbesturingssysteem](cloud-services-guestos-update-matrix.md)) voor Cloud Services worker en webservice-functies (PaaS). Dit geldt niet voor virtuele Machines (IaaS).

Microsoft heeft een gepubliceerde [ondersteuningsbeleid voor het Gastbesturingssysteem](http://support.microsoft.com/gp/azure-cloud-lifecycle-faq). De pagina die u nu wordt beschreven hoe het beleid is geïmplementeerd.

Het beleid is

1. Microsoft ondersteunt **ten minste de laatste twee families van het Gastbesturingssysteem**. Wanneer een serie buiten gebruik is gesteld, hebben klanten 12 maanden na de officiële intrekkingsdatum bij te werken naar een nieuwere ondersteunde Gast OS-familie.
2. Microsoft ondersteunt **ten minste de laatste twee versies van de ondersteunde Gastbesturingssysteem families**.
3. Microsoft ondersteunt **ten minste de laatste twee versies van de Azure SDK**. Wanneer u een versie van de SDK is buiten gebruik gesteld, moeten klanten 12 maanden na de officiële intrekkingsdatum om bij te werken naar een nieuwere versie.

Tijd tot tijd mogelijk meer dan twee families of releases worden ondersteund. Informatie over het officiële ondersteuning Gastbesturingssysteem wordt weergegeven op de [Azure Gast OS Releases en SDK compatibiliteit Matrix](cloud-services-guestos-update-matrix.md).

## <a name="when-a-guest-os-family-or-version-is-retired"></a>Wanneer een gast OS-familie of de versie is buiten gebruik gesteld
Een nieuwe Gastbesturingssysteem **familie** enige tijd opnieuw na de release van een nieuwe officiële versie van het besturingssysteem Windows Server wordt geïntroduceerd. Wanneer een nieuwe Gast OS-familie wordt geïntroduceerd, wordt de oudste Gast OS-familie intrekken van Microsoft.

Nieuwe Gastbesturingssysteem **versies** over elke maand op te nemen van de meest recente updates voor MSRC zijn geïntroduceerd. De versie van een Gastbesturingssysteem vanwege de reguliere maandelijkse updates is normaal gesproken ongeveer 60 dagen na de release uitgeschakeld. Deze activiteit houdt ten minste twee Gast OS-versies voor iedere familie beschikbaar voor gebruik.

### <a name="process-during-a-guest-os-family-retirement"></a>Verwerken tijdens een buiten gebruik stellen familie Gastbesturingssysteem
Zodra het buiten gebruik stellen wordt aangekondigd, hebben klanten een overgangsperiode van 12 maanden '' voordat de oudere familie officieel gebruik gesteld buiten. Overgangsduur van deze kan worden uitgebreid naar keuze van Microsoft. Updates worden opgeslagen op de [Azure Gast OS Releases en SDK compatibiliteit Matrix](cloud-services-guestos-update-matrix.md).

Een geleidelijke buitengebruikstellingsproces begint zes (6) maanden in de overgangsperiode. Gedurende deze tijd:

1. Microsoft zal gebruikers informeren over het buiten gebruik stellen.
2. De nieuwere versie van de Azure SDK het buiten gebruik gestelde Gast OS-familie niet wordt ondersteund.
3. Nieuwe implementaties en nieuwe distributies van Cloud Services niet toegestaan op de buiten gebruik gestelde familie

Microsoft blijft introduceren van nieuwe versie van het besturingssysteem van de Gast de meest recente updates MSRC tot de laatste dag van de overgangsperiode, ook wel de 'vervaldatum' opnemen. Op de vervaldatum wordt Cloud Services nog steeds uitgevoerd onder de SERVICEOVEREENKOMST van Azure niet te worden ondersteund. Microsoft heeft de keuze te forceren upgrade, verwijderen of stoppen van deze services na die datum.

### <a name="process-during-a-guest-os-version-retirement"></a>Verwerken tijdens een versie van het besturingssysteem Gast buiten gebruik stellen
Als klanten hun Gastbesturingssysteem automatisch bij te werken, hebben ze nooit te hoeven maken over het afhandelen van Gast OS-versies. Ze zullen altijd gebruikmaken van de meest recente versie van de Gast OS.

Gast OS-versies worden er elke maand uitgebracht. Vanwege de snelheid van reguliere releases heeft elke versie een vaste levensduur.

Na 60 dagen in de levensduur is een versie '*uitgeschakeld*'. "Uitgeschakeld" betekent dat de versie wordt verwijderd uit de portal. De versie kan niet meer worden ingesteld in het CSCFG-configuratiebestand. Bestaande implementaties worden ook actief blijven. Maar nieuwe implementaties en code en configuratie-updates aan bestaande implementaties wordt niet toegestaan.

Enige tijd na steeds "uitgeschakeld", versie van het besturingssysteem van de Gast '*verloopt*' en worden nog steeds uitgevoerd die versie installaties force bijgewerkt en ingesteld op automatisch bijwerken van het Gastbesturingssysteem in de toekomst zijn. Verlopen wordt uitgevoerd in batches, zodat de tijdsperiode van deactivering verlopen kan variëren.

Deze punten kunnen worden verricht langer goeddunken van Microsoft te vereenvoudigen overgangen van de klant. Eventuele wijzigingen worden gecommuniceerd op de [Azure Gast OS Releases en SDK compatibiliteit Matrix](cloud-services-guestos-update-matrix.md).

### <a name="notifications-during-retirement"></a>Meldingen tijdens buiten gebruik stellen
* **Familie buiten gebruik stellen** <br>Microsoft gebruikt blogberichten en de portalmelding. Klanten die nog steeds een buiten gebruik gestelde Gast OS-familie met ontvangt een melding via rechtstreekse communicatie (e-mailbericht, portal berichten, telefonische oproep) aan toegewezen servicebeheerders. Alle wijzigingen worden opgeslagen in de [Azure Gast OS Releases en SDK compatibiliteit Matrix](cloud-services-guestos-update-matrix.md).
* **Versie buiten gebruik stellen** <br>Alle wijzigingen en de datums ze optreden, worden opgeslagen in de [Azure Gast OS Releases en SDK compatibiliteit Matrix](cloud-services-guestos-update-matrix.md), met inbegrip van release, uitgeschakeld of verlopen. Beheerders van de services ontvangt e-mailberichten als ze implementaties die worden uitgevoerd op een uitgeschakelde Gastbesturingssysteem versie of een groep hebben. De timing van deze e-mailberichten kan variëren. Ze zijn doorgaans ten minste een maand voor deactivering, hoewel deze timing niet een officiële SLA is.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen
**Hoe kan ik het risico van de gevolgen van migratie?**

U wordt aangeraden nieuwste Gast OS-familie voor het ontwerpen van uw Cloud-Services.

1. Start de migratie naar een nieuwere familie vroeg plannen.
2. Tijdelijke testimplementaties instellen voor het testen van uw Cloud Service wordt uitgevoerd op de nieuwe familie.
3. Uw versie van het Gastbesturingssysteem ingesteld op **automatische** (osVersion = * in de [.cscfg](cloud-services-model-and-package.md#cscfg) bestand) zodat de migratie naar nieuwe versies van het Gastbesturingssysteem automatisch uitgevoerd.

**Wat gebeurt er als mijn webtoepassing is vereist om betere integratie met het besturingssysteem?**

Als uw web-toepassingsarchitectuur, is afhankelijk van onderliggende functies van het besturingssysteem, gebruikt u ondersteund platformmogelijkheden, zoals [starten van de taken](cloud-services-startup-tasks.md) of andere mechanismen voor uitbreidbaarheid. U kunt ook ook gebruiken [Azure Virtual Machines](https://azure.microsoft.com/documentation/scenarios/virtual-machines/) (IaaS-infrastructuur als een Service), waarbij u bent zelf verantwoordelijk voor het onderhouden van het onderliggende besturingssysteem.

## <a name="next-steps"></a>Volgende stappen
Bekijk de meest recente [Gast OS releases](cloud-services-guestos-update-matrix.md).
