---
title: Instellingen voor beveiligingsbeleid van Azure Security Center | Microsoft Docs
description: Configureer instellingen voor beveiligingsbeleid van Azure Security Center.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: f24b1e4a-cc36-4542-b21e-041453cdfcd8
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/3/2018
ms.author: rkarlin
ms.openlocfilehash: 3d2687f56b69174fde783060d994e3c01763ec94
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51256626"
---
# <a name="security-policy-settings"></a>Instellingen voor beveiligingsbeleid
Dit artikel bevat een overzicht van de beveiliging beleidsinstellingen in Security Center.

## <a name="what-are-security-policies"></a>Wat is beveiligingsbeleid?
Een beveiligingsbeleid definieert de gewenste configuratie van uw workloads en helpt ervoor te zorgen dat aan de beveiligingsvereisten van het bedrijf of aan regelgeving wordt voldaan. U kunt in Azure Security Center definieert beleid voor uw Azure-abonnementen en past u dit aan uw type workload of de vertrouwelijkheid van uw gegevens. Toepassingen met gereglementeerde gegevens, zoals persoonsgegevens, kunnen bijvoorbeeld vereisen dat een hoger beveiligingsniveau dan andere werkbelastingen.

U kunt het volgende onder beveiligingsbeleid instellen:

- **Het verzamelen van gegevens**: Hiermee bepaalt u de configuratie van agent en [gegevensverzameling](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection) instellingen.
- **Beveiligingsbeleid**: Hiermee bepaalt u welke besturingselementen Security Center bewaakt en wordt aanbevolen. U kunt bewerken de [beveiligingsbeleid](security-center-policies.md) in Security Center. U kunt ook [Azure Policy](security-center-azure-policy.md) voor het maken van nieuwe definities, definiëren van extra beleidsregels en toewijzen van beleidsregels binnen beheergroepen. 
- **E-mailmeldingen**: Hiermee bepaalt u contactpersonen voor beveiliging, en [e-mailmelding](security-center-provide-security-contact-details.md) instellingen.
- **Prijscategorie**: gratis of standard definieert [prijsselectie](security-center-pricing.md). De laag die u kiest, bepaalt welke Security Center-functies beschikbaar zijn voor resources binnen het bereik. U kunt een laag voor abonnementen, resourcegroepen en werkruimten.

> [!NOTE]
> U kunt al deze waarden per abonnement instellen. Voor werkruimten, kunt u alleen de gegevensverzameling en -prijscategorie instellen. U kunt alleen prijscategorie instellen voor resourcegroepen.
>


## <a name="who-can-edit-security-policies"></a>Wie kan beveiligingsbeleid bewerken?
Security Center maakt gebruik van rollen gebaseerd toegangsbeheer (RBAC), waarmee u ingebouwde rollen die kunnen worden toegewezen aan gebruikers, groepen en services in Azure. Wanneer gebruikers Security Center openen, zien ze alleen informatie met betrekking tot ze toegang tot hebben resources. Wat betekent dat gebruikers de rol van toegewezen *eigenaar*, *Inzender*, of *lezer* aan de groep abonnement of resourcegroep waartoe een resource behoort. Naast deze rollen zijn er twee specifieke Security Center-rollen:

- **Beveiligingslezer**: hebben weergave rechten voor Security Center, waaronder aanbevelingen, waarschuwingen, beleid en de gezondheid van, maar ze niet wijzigen.
- **Beveiligingsbeheerder**: hebben de dezelfde rechten weergeven als *beveiligingslezer*, en ze kunnen ook het beveiligingsbeleid bijwerken en verwijderen van aanbevelingen en waarschuwingen.


## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd over het beveiligingsbeleid in Azure Security Center. Zie de volgende artikelen voor meer informatie over Azure Security Center:

* [Beveiligingsbeleid instellen in Azure Security Center](security-center-policies.md): informatie over het configureren van beveiligingsbeleid voor uw Azure-abonnementen en resourcegroepen.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md): Leer hoe aanbevelingen van Security Center helpen u uw Azure-resources te beveiligen.
* [Beveiligingsstatus controleren in Azure Security Center](security-center-monitoring.md): meer informatie over het controleren van de status van uw Azure-resources.
* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen controleren met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt controleren.
- [Beveiliging van Azure Security Center gegevens](security-center-data-security.md): meer informatie over hoe Security Center beheert en beschermt gegevens.
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](https://blogs.msdn.com/b/azuresecurity/): ophalen van de meest recente Azure-beveiliging-nieuws en informatie.
