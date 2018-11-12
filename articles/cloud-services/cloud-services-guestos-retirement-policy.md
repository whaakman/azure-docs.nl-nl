---
title: Ondersteuning en buiten gebruik stellen beleid-handleiding voor Azure-Gastbesturingssysteem | Microsoft Docs
description: Bevat informatie over wat Microsoft wordt ondersteund met betrekking tot op het Azure-Gastbesturingssysteem die worden gebruikt door Cloud Services.
services: cloud-services
documentationcenter: na
author: raiye
manager: timlt
editor: ''
ms.assetid: 919dd781-4dc6-4e50-bda8-9632966c5458
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 9/20/2017
ms.author: raiye
ms.openlocfilehash: 6068f054a2ce695a889351b1f959319c64eb73fd
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51235595"
---
# <a name="azure-guest-os-supportability-and-retirement-policy"></a>Azure Guest OS-beleid voor ondersteuning en buiten gebruik stellen
De informatie op deze pagina is gekoppeld aan het Azure-gastbesturingssysteem ([Guest OS](cloud-services-guestos-update-matrix.md)) voor Cloud Services worker en webservice-rollen (PaaS). Dit geldt niet voor virtuele Machines (IaaS).

Microsoft heeft een gepubliceerde [ondersteuningsbeleid voor het Gastbesturingssysteem](https://support.microsoft.com/gp/azure-cloud-lifecycle-faq). De pagina die u leest nu wordt beschreven hoe het beleid is geïmplementeerd.

Het beleid is

1. Microsoft biedt ondersteuning voor **ten minste de laatste twee families van het Gastbesturingssysteem**. Wanneer een productfamilie is buiten gebruik gesteld, hebben klanten 12 maanden vanaf de officiële vervaldatum bij te werken naar een nieuwere ondersteunde Guest OS family.
2. Microsoft biedt ondersteuning voor **ten minste de laatste twee versies van de ondersteunde Gastbesturingssysteem families**.
3. Microsoft biedt ondersteuning voor **ten minste de laatste twee versies van de Azure SDK**. Wanneer u een versie van de SDK is buiten gebruik gesteld, wordt klanten 12 maanden vanaf de datum van de officiële buiten gebruik stellen om bij te werken naar een nieuwere versie hebben.

Soms kan meer dan twee families of versies worden ondersteund. Officiële Gastbesturingssysteem ondersteunende informatie wordt weergegeven op de [Azure Guest OS Releases en SDK Compatibility Matrix](cloud-services-guestos-update-matrix.md).

## <a name="when-a-guest-os-version-is-retired"></a>Wanneer een gast-OS-versie is buiten gebruik gesteld
Nieuwe Gastbesturingssysteem **versies** over elke maand op te nemen van de meest recente MSRC-updates zijn geïntroduceerd. Een gast-OS-versie vanwege de reguliere maandelijkse updates is normaal gesproken ongeveer 60 dagen na de release uitgeschakeld. Deze activiteit wordt ten minste twee Gast-OS-versies voor elke serie beschikbaar voor gebruik.

### <a name="process-during-a-guest-os-family-retirement"></a>Verwerken tijdens een familie buitengebruikstelling van Gastbesturingssysteem
Zodra het buiten gebruik stellen is aangekondigd, hebben klanten een overgangsperiode van 12 maanden "' voordat de oudere familie officieel gebruik gesteld buiten. Overgangsduur van deze kan worden uitgebreid naar keuze van Microsoft. Updates wordt gepost op de [Azure Guest OS Releases en SDK Compatibility Matrix](cloud-services-guestos-update-matrix.md).

Een geleidelijke buitengebruikstellingsproces begint met zes (6) maanden in de overgangsperiode. Gedurende deze tijd:

1. Microsoft ontvangt klanten van de buiten gebruik stellen.
2. De nieuwere versie van de Azure SDK niet de buiten gebruik gestelde Guest OS family wordt ondersteund.
3. Nieuwe implementaties en nieuwe distributies van Cloud Services niet toegestaan voor de buiten gebruik gestelde-familie

Microsoft blijft introduceren nieuwe Guest OS-versie de nieuwste updates MSRC tot de laatste dag van de overgangsperiode, ook wel de "vervaldatum" opnemen. Op de vervaldatum wordt Cloud Services nog steeds uitgevoerd worden ondersteund onder de Azure SLA. Microsoft heeft de keuze dwingen bijwerken, verwijderen of stoppen van die services na die datum.

### <a name="process-during-a-guest-os-version-retirement"></a>Verwerken tijdens een versie van het besturingssysteem Gast buiten gebruik stellen
Als klanten hun Gastbesturingssysteem automatisch bij te werken, hebben ze nooit te hoeven maken over het afhandelen van Gast-OS-versies. Ze worden altijd worden met behulp van de meest recente versie van de Gast-besturingssysteem.

Gast OS-versies worden elke maand uitgebracht. Elke versie is vanwege de frequentie van reguliere releases, vaste geldig.

Na 60 dagen in de levensduur van een versie is '*uitgeschakeld*'. "Uitgeschakeld" betekent dat de versie is verwijderd uit de portal. De versie kan niet meer worden ingesteld in het CSCFG-configuratiebestand. Bestaande implementaties zijn actief blijven. Maar nieuwe implementaties en code en configuratie-updates aan bestaande implementaties wordt niet toegestaan.

Even na steeds "uitgeschakeld", versie van het Gast-besturingssysteem '*verloopt*' en alle installaties nog steeds uitgevoerd die versie zijn bijgewerkt en ingesteld op automatisch bijwerken van het Gastbesturingssysteem in de toekomst. Vervaldatum wordt uitgevoerd in batches, zodat de periode van deactivering verlopen kan verschillen.

Deze punten kunnen worden verricht langer goeddunken van Microsoft om de overgang van de klant. Eventuele wijzigingen zullen worden meegedeeld de [Azure Guest OS Releases en SDK Compatibility Matrix](cloud-services-guestos-update-matrix.md).

### <a name="notifications-during-retirement"></a>Meldingen tijdens het buiten gebruik stellen
* **Familie buiten gebruik stellen** <br>Microsoft gebruikt lezen in blogposts en portalmelding. Klanten die nog steeds een buiten gebruik gestelde Guest OS family worden op de hoogte gesteld via directe communicatie (e-mailbericht, portal berichten, telefonische oproep) met toegewezen servicebeheerders. Alle wijzigingen worden geboekt naar de [Azure Guest OS Releases en SDK Compatibility Matrix](cloud-services-guestos-update-matrix.md).
* **Versie buiten gebruik stellen** <br>Alle wijzigingen en de datums ze zich voordoen worden geboekt naar de [Azure Guest OS Releases en SDK Compatibility Matrix](cloud-services-guestos-update-matrix.md), met inbegrip van release, uitgeschakeld of verlopen. Services-beheerders ontvangt e-mailberichten als ze beschikken over implementaties die worden uitgevoerd op een uitgeschakelde Guest OS-versie of een groep. De timing van deze e-mailberichten kan variëren. Ze zijn in het algemeen ten minste een maand voor deactivering, hoewel dit tijdstip niet een officiële SLA is.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen
**Hoe kan ik het risico van de gevolgen van migratie?**

U wordt aangeraden dat u nieuwste Guest OS family gebruiken voor het ontwerpen van uw Cloud-Services.

1. Begin uw migratie naar een nieuwere familie vroeg met plannen.
2. Tijdelijke testimplementaties instellen om uw Cloud-Service die wordt uitgevoerd op de nieuwe serie te testen.
3. De Gast-OS-versie ingesteld op **automatische** (osVersion = * in de [.cscfg](cloud-services-model-and-package.md#cscfg) bestand), zodat de migratie naar de nieuwe versies van het Gast-besturingssysteem automatisch uitgevoerd.

**Wat gebeurt er als mijn web-App vereist diepere integratie met het besturingssysteem?**

Als uw architectuur voor webtoepassingen, is afhankelijk van onderliggende functies van het besturingssysteem, gebruikt u de mogelijkheden van platform wordt ondersteund, zoals [opstarttaken](cloud-services-startup-tasks.md) of andere mechanismen voor uitbreidbaarheid. U kunt ook u kunt ook gebruiken [Azure Virtual Machines](https://azure.microsoft.com/documentation/scenarios/virtual-machines/) (IaaS-infrastructuur als een dienst), waarbij u bent verantwoordelijk voor het onderhouden van het onderliggende besturingssysteem.

## <a name="next-steps"></a>Volgende stappen
Bekijk de meest recente [releases van Gastbesturingssystemen](cloud-services-guestos-update-matrix.md).
