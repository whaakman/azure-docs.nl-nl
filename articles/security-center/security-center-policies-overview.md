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
ms.openlocfilehash: 7a4d056fa71f211dea5e7be649c39034eabc9395
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="security-policies-overview"></a>Overzicht van de beleidsregels voor beveiliging
Dit artikel bevat een overzicht van beveiligingsbeleid in Security Center.

## <a name="what-are-security-policies"></a>Wat is beveiligingsbeleid?
Een beveiligingsbeleid definieert de gewenste configuratie van uw workloads en helpt ervoor te zorgen dat aan de beveiligingsvereisten van het bedrijf of aan regelgeving wordt voldaan. U kunt beleid definiëren voor uw Azure-abonnementen en het beleid aanpassen aan uw type werkbelasting of de vertrouwelijkheid van uw gegevens in Azure Security Center. Toepassingen met gereglementeerde gegevens, zoals persoonlijk herleidbare informatie kunnen bijvoorbeeld vereisen dat een hoger niveau van beveiliging dan andere werkbelastingen. 

Beleidsregels van Security Center bevatten de volgende onderdelen:

- **Gegevensverzameling**: bepaalt agent inrichting en [gegevensverzameling](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection) instellingen.
- **Beveiligingsbeleid voor**: die bepaalt Security Center monitors en raadt bepaalt. U kunt bewerken de [beveiligingsbeleid](https://docs.microsoft.com/azure/security-center/security-center-policies) in Security Center. U kunt ook [Azure beleid](security-center-azure-policy.md) (in de beperkte preview) voor het maken van nieuwe definities extra beleidsregels definiëren en beleid binnen beheergroepen toe te wijzen.
- **E-mailmeldingen**: beveiliging contactpersonen, bepaalt en [e-mailmelding](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details) instellingen.
- **Prijscategorie**: gratis of standaard definieert [prijzen selectie](https://docs.microsoft.com/azure/security-center/security-center-pricing). De laag die u kiest, bepaalt welke Security Center-functies zijn beschikbaar voor resources binnen het bereik. U kunt een laag voor abonnementen en resourcegroepen werkruimten opgeven. 


## <a name="who-can-edit-security-policies"></a>Wie kan beveiligingsbeleid bewerken?
Security Center gebruikt op rollen gebaseerde toegangsbeheer (RBAC), waarmee de ingebouwde rollen die kunnen worden toegewezen aan gebruikers, groepen en -services in Azure. Wanneer gebruikers Security Center openen, zien ze alleen informatie met betrekking tot ze toegang tot hebben bronnen. Wat betekent dat gebruikers de rol van zijn toegewezen *eigenaar*, *Inzender*, of *lezer* aan het abonnement of resourcegroep groep die een bron deel uitmaakt. Naast deze rollen zijn er twee specifieke Security Center-rollen:

- **Beveiliging lezer**: hebben weergave rights met Security Center, waaronder aanbevelingen, waarschuwingen, beleid en status, maar ze niet wijzigen.
- **Beveiliging admin**: hebben de dezelfde rechten weergeven als *beveiliging lezer*, en ze kunnen ook bijwerken van het beveiligingsbeleid en aanbevelingen en waarschuwingen te negeren.


## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd over beveiligingsbeleid in Azure Security Center. Zie voor meer informatie over Azure Security Center, de volgende artikelen:

* [Beveiligingsbeleid instellen in Azure Security Center](security-center-policies.md): informatie over het beveiligingsbeleid voor uw Azure-abonnementen en resourcegroepen configureren.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md): Leer hoe aanbevelingen Beveiligingscentrum u uw Azure-resources te beveiligen.
* [Beveiligingsstatus controleren in Azure Security Center](security-center-monitoring.md): meer informatie over het controleren van de status van uw Azure-resources.
* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen controleren met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt controleren.
- [Beveiliging van gegevens van Azure Security Center](security-center-data-security.md): meer informatie over hoe Security Center beheert en beveiligt gegevens.
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/): Lees het laatste nieuws van de Azure-beveiliging en de informatie.


