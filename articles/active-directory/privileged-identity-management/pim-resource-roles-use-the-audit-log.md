---
title: Resource-rollen voor Azure-resources met behulp van Privileged Identity Management controleren | Microsoft Docs
description: Wordt uitgelegd hoe u een overzicht van alle activiteiten van de rol voor het een bepaalde resource.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 8f328a609d696886ed452589d3cdfb5f45aec62a
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39621280"
---
# <a name="audit-resource-roles-for-azure-resources-by-using-privileged-identity-management"></a>Resource-rollen voor Azure-resources controleren met behulp van Privileged Identity Management 

Resourcecontrole geeft u een overzicht van alle activiteiten van de rol voor de resource. U kunt de informatie die met behulp van een vooraf gedefinieerde datum of aangepast datumbereik filteren.
![Gegevens filteren](media/azure-pim-resource-rbac/rbac-resource-audit.png)

Resourcecontrole biedt ook snel toegang tot gegevens van de activiteit van een gebruiker. Onder **controletype**, selecteer **activeren**. Selecteer **(activiteit)** om te zien van acties van die gebruiker in Azure-resources.
![Detail van activiteit](media/azure-pim-resource-rbac/rbac-audit-activity.png)

![Meer details van de activiteit](media/azure-pim-resource-rbac/rbac-audit-activity-details.png)

# <a name="my-audit"></a>Mijn controle

Mijn controle biedt u een overzicht van de gebruiker persoonlijke rol activiteit. U kunt de informatie die met behulp van een vooraf gedefinieerde datum of aangepast datumbereik filteren.
![Persoonlijke rol activiteit](media/azure-pim-resource-rbac/my-audit-time.png)

## <a name="view-activation-and-azure-resource-activity"></a>Activering en de activiteit Azure-resources weergeven

Als u wilt zien welke acties die een specifieke gebruiker in verschillende resources heeft, kunt u de activiteit Azure-resource die is gekoppeld aan een bepaalde activeringsperiode bekijken. Begin met het selecteren van een gebruiker vanuit de **leden** weergave of uit de lijst met leden in een specifieke rol. Het resultaat wordt een grafische weergave van acties van de gebruiker weergegeven in de Azure-resources op datum. U ziet ook de recente rolactiveringen diezelfde periode van tijd.

![Gebruikersdetails](media/azure-pim-resource-rbac/rbac-user-details.png)

Selecteren van een specifieke rol moet worden geactiveerd, ziet u de details van rolactivering en de bijbehorende Azure-resource-activiteit die is opgetreden bij die gebruiker actief is.

![Selecteer de rolactivering](media/azure-pim-resource-rbac/rbac-user-resource-activity.png)

