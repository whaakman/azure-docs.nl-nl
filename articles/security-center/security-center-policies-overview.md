---
title: Azure Security Center-instellingen | Microsoft Docs
description: Azure Security Center-instellingen configureren.
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
ms.openlocfilehash: fa9b6821e1e35f7631907a029a2576d5949ac6d3
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53339634"
---
# <a name="security-center-settings"></a>Instellingen van Security Center
Dit artikel bevat een overzicht van instellingen in Security Center.

De volgende instellingen kunnen worden bereikt onder beveiligingsbeleid:

- **Het verzamelen van gegevens**: Hiermee bepaalt u de configuratie van agent en [gegevensverzameling](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection) instellingen.
- **Beveiligingsbeleid**: Bepaalt welke besturingselementen Security Center bewaakt en wordt aanbevolen. U kunt bewerken de [beveiligingsbeleid](tutorial-security-policy.md) in Security Center. U kunt ook [Azure Policy](tutorial-security-policy.md) voor het maken van nieuwe definities, definiëren van extra beleidsregels en toewijzen van beleidsregels binnen beheergroepen. 
- **E-mailmeldingen**: Hiermee bepaalt u contactpersonen voor beveiliging, en [e-mailmelding](security-center-provide-security-contact-details.md) instellingen.
- **Prijscategorie**: Hiermee definieert u gratis of standard [prijsselectie](security-center-pricing.md). De laag die u kiest, bepaalt welke Security Center-functies beschikbaar zijn voor resources binnen het bereik. U kunt een laag voor abonnementen, resourcegroepen en werkruimten.

> [!NOTE]
> U kunt al deze waarden per abonnement instellen. Voor werkruimten, kunt u alleen de gegevensverzameling en -prijscategorie instellen. U kunt alleen prijscategorie instellen voor resourcegroepen.
>


## <a name="who-can-edit-security-policies"></a>Wie kan beveiligingsbeleid bewerken?
Security Center maakt gebruik van rollen gebaseerd toegangsbeheer (RBAC), waarmee u ingebouwde rollen die kunnen worden toegewezen aan gebruikers, groepen en services in Azure. Wanneer gebruikers Security Center openen, zien ze alleen informatie met betrekking tot ze toegang tot hebben resources. Wat betekent dat gebruikers de rol van toegewezen *eigenaar*, *Inzender*, of *lezer* aan de groep abonnement of resourcegroep waartoe een resource behoort. Naast deze rollen zijn er twee specifieke Security Center-rollen:

- **Beveiligingslezer**: Heeft weergaverechten voor Security Center, waaronder aanbevelingen, waarschuwingen, beleid en de gezondheid van, maar ze kunnen geen wijzigingen aanbrengen.
- **Beveiligingsbeheerder**: Hebben de dezelfde rechten weergeven als *beveiligingslezer*, en ze kunnen ook het beveiligingsbeleid bijwerken en verwijderen van aanbevelingen en waarschuwingen.


## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd over het beveiligingsbeleid in Azure Security Center. Zie de volgende artikelen voor meer informatie over Azure Security Center:

* [Beveiligingsbeleid instellen in Azure Security Center](tutorial-security-policy.md): Informatie over het configureren van beveiligingsbeleid voor uw Azure-abonnementen en resourcegroepen.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md): Leer hoe aanbevelingen van Security Center helpen u uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md): Meer informatie over het controleren van de status van uw Azure-resources.
* [Beheren en erop reageren op beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md): Informatie over het beheren van en reageren op beveiligingswaarschuwingen.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): Meer informatie over het bewaken van de status van uw partneroplossingen.
- [Beveiliging van Azure Security Center gegevens](security-center-data-security.md): Ontdek hoe Security Center beheert en beschermt gegevens.
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md): Krijg antwoorden op veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](https://blogs.msdn.com/b/azuresecurity/): Lees het laatste nieuws van Azure-beveiliging en informatie.
