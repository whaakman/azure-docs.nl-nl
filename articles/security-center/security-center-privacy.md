---
title: Beheren van gebruikersgegevens in Azure Security Center | Microsoft Docs
description: " Informatie over het beheren van gebruikersgegevens in Azure Security Center. "
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2018
ms.author: terrylan
ms.openlocfilehash: 2495bae5c63cdafe049ec39f71ab53106c5f2df7
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655230"
---
# <a name="manage-user-data-in-azure-security-center"></a>Gebruikersgegevens in Azure Security Center beheren
In dit artikel bevat informatie over hoe u de gebruikersgegevens in Azure Security Center kunt beheren. Gebruikersgegevens beheren, biedt de mogelijkheid te openen, verwijderen of gegevens worden geëxporteerd.

[!INCLUDE [gdpr-intro-sentence.md](../../includes/gdpr-intro-sentence.md)]

Security Center van een gebruiker de rol van de lezer, eigenaar, bijdrager of accountbeheerder toegang tot gegevens in het hulpprogramma van de klant. Zie [ingebouwde functies voor op rollen gebaseerd toegangsbeheer van Azure](../role-based-access-control/built-in-roles.md) voor meer informatie over de rollen lezer, de eigenaar en Inzender. Zie [Azure-abonnementbeheerders](../billing/billing-add-change-azure-subscription-administrator.md) voor meer informatie over de accountbeheerder-rol.

## <a name="searching-for-and-identifying-personal-data"></a>Zoeken naar en persoonlijke gegevens worden geïdentificeerd
Een gebruiker Security Center kan de persoonlijke gegevens via de Azure portal bekijken. Security Center bewaart alleen de contactgegevens van beveiliging zoals e-mailadressen en telefoonnummers. Zie [contact op met informatie over de beveiliging in Azure Security Center bieden](security-center-provide-security-contact-details.md) voor meer informatie.

Een gebruiker kan toegestane IP-configuraties van het Beveiligingscentrum met NET in tijd VM toegang functie weergeven in de Azure portal. Zie [VM-toegang beheren met behulp van just-in-time](security-center-just-in-time.md) voor meer informatie.

Een gebruiker kan beveiligingswaarschuwingen geleverd door Security Center, inclusief IP-adressen en aanvaller details weergeven in de Azure portal. Zie [beheren en erop reageren beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md) voor meer informatie.

## <a name="classifying-personal-data"></a>Persoonlijke gegevens classificeren
U hoeft niet te classificeren van persoonlijke gegevens gevonden in contact op met beveiligingsfunctie van Security Center. De opgeslagen gegevens is een e-mailadres (of meerdere e-mailadressen) en een telefoonnummer. [Neem contact op met de gegevens](security-center-provide-security-contact-details.md) wordt gevalideerd door Security Center.

U hoeft niet te classificeren van de IP-adressen en poortnummers die zijn opgeslagen door Security Center van [in de tijd](security-center-just-in-time.md) functie.

Alleen een gebruiker aan de rol van systeembeheerder is toegewezen, kunnen classificeren persoonlijke gegevens door [waarschuwingen weergeven](security-center-managing-and-responding-alerts.md) in Security Center.

## <a name="securing-and-controlling-access-to-personal-data"></a>Beveiligen en beheren van toegang tot persoonlijke gegevens
Security Center van een gebruiker de rol van de lezer, eigenaar, bijdrager of accountbeheerder toegang tot [contact op met beveiligingsgegevens](security-center-provide-security-contact-details.md).

Security Center van een gebruiker de rol van de lezer, eigenaar, bijdrager of accountbeheerder toegang tot hun [in de tijd](security-center-just-in-time.md) beleid.

Security Center van een gebruiker de rol van de lezer, eigenaar, bijdrager of accountbeheerder kunt bekijken hun [waarschuwingen](security-center-managing-and-responding-alerts.md).

## <a name="updating-personal-data"></a>Bijwerken van persoonlijke gegevens
Security Center van een gebruiker de rol van eigenaar, bijdrager, toegewezen of accountbeheerder kunt bijwerken [contact op met beveiligingsgegevens](security-center-provide-security-contact-details.md) via de Azure-portal.

Security Center van een gebruiker de rol van eigenaar, bijdrager, toegewezen of accountbeheerder kunt bijwerken hun [NET in tijd beleidsregels](security-center-just-in-time.md).

Een beheerder Account bewerken niet waarschuwing incidenten. Een [waarschuwing incident](security-center-managing-and-responding-alerts.md) wordt beschouwd als beveiligingsgegevens en alleen-lezen is.

## <a name="deleting-personal-data"></a>Verwijderen van persoonlijke gegevens
Security Center van een gebruiker de rol van eigenaar, bijdrager, toegewezen of accountbeheerder kunt verwijderen [contact op met beveiligingsgegevens](security-center-provide-security-contact-details.md) via de Azure-portal.

Security Center van een gebruiker de rol van eigenaar, bijdrager, toegewezen of accountbeheerder kunt verwijderen de [NET in tijd beleidsregels](security-center-just-in-time.md) via de Azure-portal.

Een gebruiker Security Center alert incidenten kan niet verwijderen. Als gevolg van beveiligingsbehoeften, een [waarschuwing incident](security-center-managing-and-responding-alerts.md) wordt beschouwd als gelezen alleen gegevens.

## <a name="exporting-personal-data"></a>Exporteren van persoonlijke gegevens
Security Center van een gebruiker de rol van de lezer, eigenaar, bijdrager of accountbeheerder kunt exporteren [contact op met beveiligingsgegevens](security-center-provide-security-contact-details.md) door:

- Uitvoeren van een kopie van de Azure-portal
- Uitvoeren van de Azure REST-API-aanroep, HTTP-ophalen:
```HTTP
GET https://<endpoint>/subscriptions/{subscriptionId}/providers/Microsoft.Security/securityContacts?api-version={api-version}
```

Een gebruiker Security Center is de rol van de accountbeheerder toegewezen kunt exporteren de [NET in tijd beleidsregels](security-center-just-in-time.md) door met het IP-adres adressen:

- Uitvoeren van een kopie van de Azure-portal
- Uitvoeren van de Azure REST-API-aanroep, HTTP-ophalen:
```HTTP
GET https://<endpoint>/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Security/locations/{location}/jitNetworkAccessPolicies/default?api-version={api-version}
```

Een beheerder Account kunt de details van de waarschuwing door exporteren:

- Uitvoeren van een kopie van de Azure-portal
- Uitvoeren van de Azure REST-API-aanroep, HTTP-ophalen:
```HTTP
GET https://<endpoint>/subscriptions/{subscriptionId}/providers/microsoft.Security/alerts?api-version={api-version}
```

Zie [beveiligingswaarschuwingen ophalen (verzameling ophalen)](https://msdn.microsoft.com/library/mt704050.aspx) voor meer informatie.

## <a name="restricting-the-use-of-personal-data-for-profiling-or-marketing-without-consent"></a>Het gebruik van persoonlijke gegevens voor profilering of marketing zonder toestemming beperken
Security Center kunt u kiezen meedoen door het verwijderen van hun [contact op met beveiligingsgegevens](security-center-provide-security-contact-details.md).

[Alleen bij tijdgegevens](security-center-just-in-time.md) wordt beschouwd als niet-herleidbare informatie en een periode van 30 dagen bewaard.

[Waarschuwingsgegevens](security-center-managing-and-responding-alerts.md) wordt beschouwd als beveiligingsgegevens en gedurende een periode van twee jaar worden bewaard.

## <a name="auditing-and-reporting"></a>Controle en rapportage
Controlelogboeken van contactpersoon voor beveiliging, in de tijd en updates worden bijgehouden in een waarschuwing [Azure activiteitenlogboeken](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het beheren van gebruikersgegevens [gevonden in Azure Security Center onderzoek gebruikersgegevens beheren](security-center-investigation-user-data.md).
