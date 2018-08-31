---
title: Controlegeschiedenis voor Azure resource-rollen in PIM weergeven | Microsoft Docs
description: Informatie over het weergeven van controlegeschiedenis voor Azure resource-rollen in Azure AD Privileged Identity Management (PIM).
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
ms.openlocfilehash: c0536423e9640f78149b612ec66b0a07cdcf24bb
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189308"
---
# <a name="view-audit-history-for-azure-resource-roles-in-pim"></a>Controlegeschiedenis voor Azure-resource-rollen in PIM weergeven

Resourcecontrole geeft u een overzicht van alle activiteiten van de rol voor de resource. U kunt de informatie die met behulp van een vooraf gedefinieerde datum of aangepast datumbereik filteren.
![Gegevens filteren](media/azure-pim-resource-rbac/rbac-resource-audit.png)

Resourcecontrole biedt ook snel toegang tot gegevens van de activiteit van een gebruiker. Onder **controletype**, selecteer **activeren**. Selecteer **(activiteit)** om te zien van acties van die gebruiker in Azure-resources.
![Detail van activiteit](media/azure-pim-resource-rbac/rbac-audit-activity.png)

![Meer details van de activiteit](media/azure-pim-resource-rbac/rbac-audit-activity-details.png)

## <a name="my-audit"></a>Mijn controle

Mijn controle biedt u een overzicht van de gebruiker persoonlijke rol activiteit. U kunt de informatie die met behulp van een vooraf gedefinieerde datum of aangepast datumbereik filteren.
![Persoonlijke rol activiteit](media/azure-pim-resource-rbac/my-audit-time.png)

## <a name="view-activation-and-azure-resource-activity"></a>Activering en Azure-resource-activiteit bekijken

Als u wilt zien welke acties die een specifieke gebruiker in verschillende resources heeft, kunt u de activiteit Azure-resource die is gekoppeld aan een bepaalde activeringsperiode bekijken. Begin met het selecteren van een gebruiker vanuit de **leden** weergave of uit de lijst met leden in een specifieke rol. Het resultaat wordt een grafische weergave van acties van de gebruiker weergegeven in de Azure-resources op datum. U ziet ook de recente rolactiveringen diezelfde periode van tijd.

![Gebruikersdetails](media/azure-pim-resource-rbac/rbac-user-details.png)

Selecteren van een specifieke rol moet worden geactiveerd, ziet u de details van rolactivering en de bijbehorende Azure-resource-activiteit die is opgetreden bij die gebruiker actief is.

![Selecteer de rolactivering](media/azure-pim-resource-rbac/rbac-user-resource-activity.png)

## <a name="next-steps"></a>Volgende stappen

- [Controlegeschiedenis voor Azure AD directory-rollen in PIM weergeven](pim-how-to-use-audit-log.md)
