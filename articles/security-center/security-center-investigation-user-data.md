---
title: Beheren van gegevens van de gebruiker gevonden in een onderzoek in Azure Security Center | Microsoft Docs
description: " Informatie over het beheren van gegevens van de gebruiker gevonden in de onderzoeksfunctie van Azure Security Center. "
services: operations-management-suite
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: operations-management-suite
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/20/2018
ms.author: rkarlin
ms.openlocfilehash: bec9be627937913c268d65bb6cdadcf68c7a3fb4
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57244295"
---
# <a name="manage-user-data-found-in-an-azure-security-center-investigation"></a>Gebruikersgegevens gevonden in een onderzoek in Azure Security Center beheren
In dit artikel bevat informatie over het beheren van de gebruikersgegevens gevonden in de onderzoeksfunctie van Azure Security Center. Onderzoek gegevens worden opgeslagen in [logboeken van Azure Monitor](../log-analytics/log-analytics-overview.md) en zichtbaar gemaakt in Security Center. Gebruikersgegevens beheren, biedt de mogelijkheid om te verwijderen of exporteren van gegevens.

[!INCLUDE [gdpr-intro-sentence.md](../../includes/gdpr-intro-sentence.md)]

## <a name="searching-for-and-identifying-personal-data"></a>Zoeken naar en het identificeren van persoonlijke gegevens
In de Azure-portal, kunt u Security Center [onderzoeksfunctie](../security-center/security-center-investigation.md) om te zoeken naar persoonlijke gegevens. De onderzoeksfunctie bevindt zich onder **beveiligingswaarschuwingen**.

De onderzoeksfunctie toont alle entiteiten, gebruikersgegevens en gegevens onder de **entiteiten** tabblad.

## <a name="securing-and-controlling-access-to-personal-information"></a>Beveiligen en beheren van toegang tot persoonlijke gegevens
Een gebruiker met Security Center toegewezen de rol van lezer, eigenaar, bijdrager of accountbeheerder toegang heeft tot klantgegevens in het hulpprogramma.

Zie [ingebouwde rollen voor op rollen gebaseerd toegangsbeheer in Azure](../role-based-access-control/built-in-roles.md) voor meer informatie over de rollen lezer, eigenaar en Inzender. Zie [Azure-abonnementbeheerders](../billing/billing-add-change-azure-subscription-administrator.md) voor meer informatie over de accountbeheerder-rol.

## <a name="deleting-personal-data"></a>Het verwijderen van persoonlijke gegevens
Security Center van een gebruiker de rol van eigenaar, Inzender, toegewezen of beheerder van de informatie voor onderzoek kunt verwijderen.

Als u wilt verwijderen een onderzoek, kunt u indienen een `DELETE` aanvraag naar de Azure Resource Manager REST API:

```HTTP
DELETE
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}/features/security/incidents/{incidentName}
```

De `incidentName` invoer kan worden gevonden door de lijst van alle incidenten met behulp van een `GET` aanvraag:

```HTTP
GET
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}/features/security/incidents
```

## <a name="exporting-personal-data"></a>Exporteren van persoonlijke gegevens
Security Center van een gebruiker de rol van eigenaar, Inzender, toegewezen of beheerder van de informatie voor onderzoek kunt exporteren. Als u wilt exporteren informatie voor onderzoek, gaat u naar de **entiteiten** tabblad kopiëren en plakken van de relevante informatie.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het beheren van gebruikersgegevens [gebruikersgegevens in Azure Security Center beheren](security-center-privacy.md).
Zie voor meer informatie over het verwijderen van persoonlijke gegevens in Azure Monitor-Logboeken, [exporteren en verwijderen van persoonlijke gegevens](../azure-monitor/platform/personal-data-mgmt.md#how-to-export-and-delete-private-data).
