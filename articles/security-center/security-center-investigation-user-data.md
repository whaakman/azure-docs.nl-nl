---
title: Gebruikersgegevens gevonden in Azure Security Center onderzoek beheren | Microsoft Docs
description: " Informatie over het beheren van gegevens van de gebruiker in Azure Security Center onderzoek functie gevonden. "
services: operations-management-suite
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2018
ms.author: terrylan
ms.openlocfilehash: 6685db4eeda72928753c74c64b4b26539ccb1eb9
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655226"
---
# <a name="manage-user-data-found-in-an-azure-security-center-investigation"></a>Gebruikersgegevens gevonden in Azure Security Center onderzoek beheren
In dit artikel bevat informatie over het beheren van de gebruikersgegevens gevonden in Azure Security Center onderzoek functie. Onderzoek gegevens worden opgeslagen in [Azure Log Analytics](../log-analytics/log-analytics-overview.md) en zijn beschikbaar in Security Center. Gebruikersgegevens beheren, biedt de mogelijkheid om te verwijderen of gegevens worden geëxporteerd.

[!INCLUDE [gdpr-intro-sentence.md](../../includes/gdpr-intro-sentence.md)]

## <a name="searching-for-and-identifying-personal-data"></a>Zoeken naar en persoonlijke gegevens worden geïdentificeerd
In de Azure portal kunt u de Security Center [onderzoek functie](../security-center/security-center-investigation.md) om te zoeken naar persoonlijke gegevens. De functie onderzoek is beschikbaar onder **beveiligingswaarschuwingen**.

De functie onderzoek toont alle entiteiten, gebruikersgegevens en -gegevens onder de **entiteiten** tabblad.

## <a name="securing-and-controlling-access-to-personal-information"></a>Beveiligen en beheren van toegang tot persoonlijke gegevens
Security Center van een gebruiker de rol van de lezer, eigenaar, bijdrager of accountbeheerder toegang tot gegevens in het hulpprogramma van de klant.

Zie [ingebouwde functies voor op rollen gebaseerd toegangsbeheer van Azure](../role-based-access-control/built-in-roles.md) voor meer informatie over de rollen lezer, de eigenaar en Inzender. Zie [Azure-abonnementbeheerders](../billing/billing-add-change-azure-subscription-administrator.md) voor meer informatie over de accountbeheerder-rol.

## <a name="deleting-personal-data"></a>Verwijderen van persoonlijke gegevens
Security Center van een gebruiker de rol van eigenaar, bijdrager, toegewezen of accountbeheerder onderzoek gegevens kunt verwijderen.

Als u wilt verwijderen een onderzoek, dient u een `DELETE` aanvraag voor de REST-API van Azure Resource Manager:

```HTTP
DELETE
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}/features/security/incidents/{incidentName}
```

De `incidentName` invoer vindt lijst met alle incidenten met een `GET` aanvraag:

```HTTP
GET
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}/features/security/incidents
```

## <a name="exporting-personal-data"></a>Exporteren van persoonlijke gegevens
Security Center van een gebruiker de rol van eigenaar, bijdrager, toegewezen of de beheerder van de informatie onderzoek kunt exporteren. Als u wilt exporteren onderzoek informatie, gaat u naar de **entiteiten** tabblad kopiëren en plakken van de relevante informatie.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het beheren van gebruikersgegevens [gebruikersgegevens in Azure Security Center beheren](security-center-privacy.md).
