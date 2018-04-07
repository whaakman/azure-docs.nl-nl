---
title: Privileged Identity Management voor Resources in Azure - Resource audit | Microsoft Docs
description: Wordt uitgelegd hoe u een overzicht krijgen van de rol van activiteiten voor de een bepaalde bron.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: b8fa7d5600c0de8a3319ea4de785281372959937
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-roles---audit"></a>Privileged Identity Management - resourcerollen - Audit

Controle van de resource hebt u een overzicht van de rol van activiteiten voor de resource. U kunt de gegevens door middel van een vooraf gedefinieerde datum of aangepaste datumbereik filteren.
![](media/azure-pim-resource-rbac/rbac-resource-audit.png)

Controle van de resource biedt ook snel toegang om gegevens van de activiteit van een gebruiker weer te geven. Selecteer onder 'Audit-type' 'Activeren'. Klik op '(activiteit)' van de gebruiker acties op Azure-Resources bekijken.
![](media/azure-pim-resource-rbac/rbac-audit-activity.png)

![](media/azure-pim-resource-rbac/rbac-audit-activity-details.png)

# <a name="my-audit"></a>Mijn controle

Mijn audit biedt u een weergave van een gebruiker persoonlijk rol activiteit. U kunt de gegevens door middel van een vooraf gedefinieerde datum of aangepaste datumbereik filteren.
![](media/azure-pim-resource-rbac/my-audit-time.png)

## <a name="view-activation-and-azure-resource-activity"></a>Activering en Azure Resource activiteit weergeven

In het geval u zien welke acties die een specifieke gebruiker heeft uitgevoerd op verschillende bronnen wilt, kunt u de Azure Resource-activiteit die is gekoppeld aan een bepaalde activeringsperiode (voor gebruikers in aanmerking komende) kunt bekijken. Begin met het selecteren van een gebruiker vanuit de weergave van de leden of in de lijst met leden in een specifieke functie. Het resultaat wordt een grafische weergave van de gebruiker acties op Azure-Resources op datum en de recente functie-activeringen via dezelfde periode.

![](media/azure-pim-resource-rbac/rbac-user-details.png)

Selecteren van een specifieke rol moet worden geactiveerd, ziet de rolgegevens voor activering en de bijbehorende Azure Resource-activiteit die is opgetreden tijdens het die gebruiker actief was.

![](media/azure-pim-resource-rbac/rbac-user-resource-activity.png)

