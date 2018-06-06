---
title: Resource-functies voor Azure-resources met behulp van Privileged Identity Management controleren | Microsoft Docs
description: Wordt uitgelegd hoe u een overzicht krijgen van de rol van activiteiten voor de een bepaalde bron.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 714c8ba2a6acf1fdb73fe53f9d2aaf1d98ee9abc
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34699747"
---
# <a name="audit-resource-roles-for-azure-resources-by-using-privileged-identity-management"></a>Audit resourcerollen voor Azure-resources met behulp van Privileged Identity Management 

Controle van de resource hebt u een overzicht van de rol van activiteiten voor de resource. U kunt de gegevens door middel van een vooraf gedefinieerde datum of aangepaste datumbereik filteren.
![Gegevens filteren](media/azure-pim-resource-rbac/rbac-resource-audit.png)

Controle van de resource biedt ook snel toegang tot gegevens van de activiteit van een gebruiker. Onder **type controleren**, selecteer **activeren**. Selecteer **(activiteit)** om te zien die gebruiker acties in de Azure-resources.
![Details controleactiviteit](media/azure-pim-resource-rbac/rbac-audit-activity.png)

![Meer details van de activiteit](media/azure-pim-resource-rbac/rbac-audit-activity-details.png)

# <a name="my-audit"></a>Mijn controle

Mijn audit biedt u een weergave van een gebruiker persoonlijk rol activiteit. U kunt de gegevens door middel van een vooraf gedefinieerde datum of aangepaste datumbereik filteren.
![Persoonlijke rol activiteit](media/azure-pim-resource-rbac/my-audit-time.png)

## <a name="view-activation-and-azure-resource-activity"></a>Activering en Azure Resource activiteit weergeven

Als u wilt zien welke acties die een specifieke gebruiker in verschillende bronnen heeft, kunt u de Azure-resource-activiteit die is gekoppeld aan een bepaalde activeringsperiode bekijken. Begin met het selecteren van een gebruiker vanuit de **leden** weergave of uit de lijst met leden in een specifieke functie. Het resultaat wordt een grafische weergave van acties van de gebruiker weergegeven in de Azure-resources op datum. Ook ziet u de recente functie-activeringen via dezelfde periode.

![Gebruikersdetails](media/azure-pim-resource-rbac/rbac-user-details.png)

Selecteren van een specifieke rol moet worden geactiveerd, ziet u de rolgegevens voor activering en de bijbehorende Azure-resource-activiteit die is opgetreden tijdens het die gebruiker actief was.

![Selecteer rolactivering](media/azure-pim-resource-rbac/rbac-user-resource-activity.png)

