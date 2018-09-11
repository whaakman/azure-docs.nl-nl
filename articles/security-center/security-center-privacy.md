---
title: Beheren van gebruikersgegevens in Azure Security Center | Microsoft Docs
description: " Informatie over het beheren van gegevens van de gebruiker in Azure Security Center. "
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2018
ms.author: terrylan
ms.openlocfilehash: fcaac3d248b676e4b7b1fe0344b54e52ce1fb558
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44302670"
---
# <a name="manage-user-data-in-azure-security-center"></a>Gebruikersgegevens in Azure Security Center beheren
In dit artikel bevat informatie over hoe u de gegevens van de gebruiker in Azure Security Center kunt beheren. Gebruikersgegevens beheren, biedt de mogelijkheid te openen, verwijderen of exporteren van gegevens.

[!INCLUDE [gdpr-intro-sentence.md](../../includes/gdpr-intro-sentence.md)]

Een gebruiker met Security Center toegewezen de rol van lezer, eigenaar, bijdrager of accountbeheerder toegang heeft tot klantgegevens in het hulpprogramma. Zie [ingebouwde rollen voor op rollen gebaseerd toegangsbeheer in Azure](../role-based-access-control/built-in-roles.md) voor meer informatie over de rollen lezer, eigenaar en Inzender. Zie [Azure-abonnementbeheerders](../billing/billing-add-change-azure-subscription-administrator.md) voor meer informatie over de accountbeheerder-rol.

## <a name="searching-for-and-identifying-personal-data"></a>Zoeken naar en het identificeren van persoonlijke gegevens
Een gebruiker Security Center kan de persoonlijke gegevens via Azure portal bekijken. Contactgegevens voor beveiliging, zoals e-mailadressen en telefoonnummers worden alleen opgeslagen in Security Center. Zie [bieden contactgegevens voor beveiliging in Azure Security Center](security-center-provide-security-contact-details.md) voor meer informatie.

Een gebruiker kan toegestane IP-configuraties die just-in-time VM access functie met behulp van Security Center bekijken in de Azure-portal. Zie [VM-toegang beheren met behulp van just-in-time](security-center-just-in-time.md) voor meer informatie.

Een gebruiker kan beveiligingswaarschuwingen die door Security Center, met inbegrip van IP-adressen en de details van de aanvaller bekijken in de Azure-portal. Zie [beheren en erop reageren op beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md) voor meer informatie.

## <a name="classifying-personal-data"></a>Classificatie van persoonlijke gegevens
U hoeft niet te classificeren van persoonlijke gegevens die zijn gevonden in contact op met beveiligingsfunctie van Security Center bevinden. De opgeslagen gegevens is een e-mailadres (of meerdere e-mailadressen) en een telefoonnummer. [Neem contact op met de gegevens](security-center-provide-security-contact-details.md) is gevalideerd door Security Center.

U hoeft niet te classificeren van de IP-adressen en poortnummers die zijn opgeslagen door Security Center [just-in-time](security-center-just-in-time.md) functie.

Alleen een gebruiker aan de rol van beheerder toegewezen kunt classificeren persoonlijke gegevens door [weergeven van waarschuwingen](security-center-managing-and-responding-alerts.md) in Security Center.

## <a name="securing-and-controlling-access-to-personal-data"></a>Beveiligen en beheren van toegang tot persoonlijke gegevens
Security Center van een gebruiker de rol van lezer, eigenaar, Inzender toegewezen of Account beheerder toegang [contact op met beveiligingsgegevens](security-center-provide-security-contact-details.md).

Security Center van een gebruiker de rol van lezer, eigenaar, Inzender toegewezen of accountbeheerder toegang tot hun [just-in-time](security-center-just-in-time.md) beleid.

Security Center van een gebruiker de rol van lezer, eigenaar, Inzender toegewezen of accountbeheerder vindt hun [waarschuwingen](security-center-managing-and-responding-alerts.md).

## <a name="updating-personal-data"></a>Het bijwerken van persoonlijke gegevens
Security Center van een gebruiker de rol van eigenaar, Inzender, toegewezen of accountbeheerder kunt bijwerken [contact op met beveiligingsgegevens](security-center-provide-security-contact-details.md) via Azure portal.

Security Center van een gebruiker de rol van eigenaar, Inzender, toegewezen of accountbeheerder kunnen bijwerken hun [just-in-time-beleid](security-center-just-in-time.md).

Een accountbeheerder bewerken niet waarschuwing incidenten. Een [waarschuwing incident](security-center-managing-and-responding-alerts.md) wordt beschouwd als beveiligingsgegevens en is alleen-lezen.

## <a name="deleting-personal-data"></a>Het verwijderen van persoonlijke gegevens
Accountbeheerder verwijderen of een gebruiker met Security Center de rol van eigenaar, Inzender, toegewezen [contact op met beveiligingsgegevens](security-center-provide-security-contact-details.md) via Azure portal.

Accountbeheerder verwijderen of een gebruiker Security Center toegewezen de rol van eigenaar, Inzender, de [just-in-time-beleid](security-center-just-in-time.md) via Azure portal.

Een gebruiker met Security Center kan de waarschuwing incidenten niet verwijderen. Vanwege de beveiligingsbehoeften, een [waarschuwing incident](security-center-managing-and-responding-alerts.md) wordt beschouwd als gelezen alleen gegevens.

## <a name="exporting-personal-data"></a>Exporteren van persoonlijke gegevens
Security Center van een gebruiker de rol van lezer, eigenaar, Inzender toegewezen of accountbeheerder kunt exporteren [contact op met beveiligingsgegevens](security-center-provide-security-contact-details.md) door:

- Uitvoeren van een kopie van de Azure-portal
- Uitvoeren van de Azure REST API-aanroep GET HTTP:
```HTTP
GET https://<endpoint>/subscriptions/{subscriptionId}/providers/Microsoft.Security/securityContacts?api-version={api-version}
```

Een Security Center-gebruiker die de rol van de accountbeheerder kan exporteren de [just-in-time-beleid](security-center-just-in-time.md) door het IP-adres met adressen:

- Uitvoeren van een kopie van de Azure-portal
- Uitvoeren van de Azure REST API-aanroep GET HTTP:
```HTTP
GET https://<endpoint>/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Security/locations/{location}/jitNetworkAccessPolicies/default?api-version={api-version}
```

Een accountbeheerder kan de details van de waarschuwing door te exporteren:

- Uitvoeren van een kopie van de Azure-portal
- Uitvoeren van de Azure REST API-aanroep GET HTTP:
```HTTP
GET https://<endpoint>/subscriptions/{subscriptionId}/providers/microsoft.Security/alerts?api-version={api-version}
```

Zie [beveiligingswaarschuwingen ophalen (verzameling ophalen)](https://msdn.microsoft.com/library/mt704050.aspx) voor meer informatie.

## <a name="restricting-the-use-of-personal-data-for-profiling-or-marketing-without-consent"></a>Het gebruik van persoonlijke gegevens voor profilering of marketing zonder toestemming beperken
Een gebruiker met Security Center kunt afmelden door het verwijderen van hun [contact op met beveiligingsgegevens](security-center-provide-security-contact-details.md).

[Just-in-time-gegevens](security-center-just-in-time.md) wordt beschouwd als niet-identificeerbare gegevens en worden gedurende een periode van 30 dagen bewaard.

[Waarschuwingsgegevens](security-center-managing-and-responding-alerts.md) wordt beschouwd als beveiligingsgegevens en gedurende een periode van twee jaar worden bewaard.

## <a name="auditing-and-reporting"></a>Controle en rapportage
Controlelogboeken van de contactpersoon voor beveiliging, just-in-time en updates worden bijgehouden in een waarschuwing [Azure-activiteitenlogboeken](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het beheren van gebruikersgegevens [gevonden in een Azure Security Center-onderzoek gebruikersgegevens beheren](security-center-investigation-user-data.md).
