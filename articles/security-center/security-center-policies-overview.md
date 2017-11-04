---
title: Inleiding tot Azure Security Center-beveiligingsbeleid | Microsoft Docs
description: Meer informatie over Azure Security Center-beveiligingsbeleid en de belangrijkste mogelijkheden.
services: security-center
documentationcenter: na
author: YuriDio
manager: MBaldwin
editor: 
ms.assetid: f24b1e4a-cc36-4542-b21e-041453cdfcd8
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2017
ms.author: yurid
ms.openlocfilehash: 95ef2099cb16bcfd550ce2799428f1a16031f535
ms.sourcegitcommit: 5d772f6c5fd066b38396a7eb179751132c22b681
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/13/2017
---
# <a name="security-policies-overview"></a>Overzicht van de beleidsregels voor beveiliging
Dit document bevat een overzicht van beveiligingsbeleid in Security Center.

## <a name="what-are-security-policies"></a>Wat is beveiligingsbeleid?
Een beveiligingsbeleid definieert de gewenste configuratie van uw workloads en helpt ervoor te zorgen dat aan de beveiligingsvereisten van het bedrijf of aan regelgeving wordt voldaan. In Security Center kunt u beleidsregels definiëren voor uw Azure-abonnementen, die kunnen worden afgestemd op het type workload of de vertrouwelijkheid van gegevens. Bijvoorbeeld, toepassingen die gebruikmaken van gereglementeerde gegevens zoals persoonlijk identificeerbare informatie mogelijk een hoger niveau van beveiliging dan andere werkbelastingen. 

Beleidsregels van Security Center bevatten de volgende onderdelen:

- Verzamelen van gegevens: agent wordt ingericht en [gegevensverzameling](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection) instellingen.
- Beveiligingsbeleid: bepaalt welke besturingselementen wordt gecontroleerd en wordt aanbevolen door Security Center (bewerken de [beveiligingsbeleid](https://docs.microsoft.com/en-us/azure/security-center/security-center-policies) in Security Center, of gebruik [Azure beleid](security-center-azure-policy.md), beperkte Preview-versie maken van nieuwe definities, extra beleidsregels definiëren, en het toewijzen van beleid binnen beheergroepen).
- E-mailmeldingen: beveiliging contactpersonen en [e-mailmelding](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details) instellingen.
- Prijscategorie: gratis of standaard [selectie prijzen](https://docs.microsoft.com/azure/security-center/security-center-pricing), waarmee wordt bepaald welke Security Center-functies zijn beschikbaar voor resources binnen het bereik (kan worden opgegeven voor abonnementen en resourcegroepen werkruimten). 


## <a name="who-can-edit-security-policies"></a>Wie kan beveiligingsbeleid bewerken?
Security Center gebruikt op rollen gebaseerde toegangsbeheer (RBAC), waarmee de ingebouwde rollen die kunnen worden toegewezen aan gebruikers, groepen en -services in Azure. Wanneer gebruikers Security Center openen, zien ze alleen informatie met betrekking tot resources waartoe ze toegang hebben. Dit betekent dat aan de gebruiker de rol van de eigenaar, bijdrager of lezer is toegewezen voor het abonnement of de resourcegroep waartoe een resource behoort. Naast deze rollen zijn er twee specifieke Security Center-rollen:

- Beveiliging lezer: gebruiker die deel uitmaakt van deze rol kan rechten met Security Center, waaronder aanbevelingen, waarschuwingen, beleid en status, weergeven, maar niet mogelijk wijzigingen aanbrengen.
- De beheerder beveiliging: zelfde als lezer van de beveiliging, maar kan ook het beveiligingsbeleid, bijwerken sluiten aanbevelingen en waarschuwingen.


## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd over beveiligingsbeleid in Azure Security Center. Zie de volgende onderwerpen voor meer informatie over Azure Security Center:

* [Beveiligingsbeleid instellen in Azure Security Center](security-center-policies.md) : informatie over het beveiligingsbeleid voor uw Azure-abonnementen en resourcegroepen configureren.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md) : Leer hoe aanbevelingen u uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md): meer informatie over het bewaken van de status van uw Azure-resources.
* [Het beheren van en reageren op beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md) : informatie over het beheren van en reageren op beveiligingswaarschuwingen.
* [Partneroplossingen controleren met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt controleren.
- [Beveiliging van gegevens van Azure Security Center](security-center-data-security.md) -informatie over hoe gegevens worden beheerd en beveiligd in Security Center.
* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/) — Lees het laatste nieuws van de Azure-beveiliging en de informatie.


