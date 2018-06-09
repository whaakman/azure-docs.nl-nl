---
title: Toegang beoordelingen uitvoeren in Azure-resources met behulp van Privileged Identity Management | Microsoft Docs
description: Wordt uitgelegd hoe u een access-revisie starten in Privileged Identity Management voor Azure-Resources
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.component: protection
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: b5c2f13386a996a6c7895bd4755b6cf609a5df72
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35233251"
---
# <a name="perform-access-reviews-in-azure-resources-by-using-privileged-identity-management"></a>Toegang beoordelingen uitvoeren in Azure-resources met behulp van Privileged Identity Management
Roltoewijzingen worden 'verouderde' wanneer gebruikers uitgebreide toegang dat ze niet meer nodig hebt. Als u het risico dat is gekoppeld aan deze verouderde roltoewijzingen, neem bevoorrechte rol beheerders regelmatig rollen. Dit document worden de stappen voor het starten van een onderzoek toegang in Privileged Identity Management (PIM) voor Azure-resources.

Ga naar de PIM-toepassing hoofdpagina van:

* **Toegang tot beoordelingen** > **toevoegen**

![Beoordelingen toegang toevoegen](media/azure-pim-resource-rbac/rbac-access-review-home.png)

Wanneer u selecteert de **toevoegen** knop, de **maken een bekijken toegang** blade wordt weergegeven. De controle configureren met een naam en een tijdslimiet, kiest u een rol om te controleren en vervolgens besluit wie doet de controle op deze blade.

![Een toegangsbeoordeling maken](media/azure-pim-resource-rbac/rbac-create-access-review.png)

### <a name="configure-the-review"></a>De controle configureren
Eerst naam voor het maken van een toegang controleren en stel een begin- en datum.

![Configureren van de evaluatie - schermafbeelding](media/azure-pim-resource-rbac/rbac-access-review-setting-1.png)

Controleer de lengte van de evaluatie lang genoeg is voor gebruikers om deze te voltooien. Als ze wordt voltooid voordat de einddatum, kunnen ze de controle altijd voortijdig gestopt.

### <a name="choose-a-role-to-review"></a>Kies een rol te bekijken
Iedere evaluatie is gericht op slechts één rol. Tenzij u de controle van toegang vanuit een specifieke rolblade hebt gestart, moet u nu een rol kiezen.

1. Ga naar **lidmaatschap van de gebruikersrol controleren**
   
    ![Bekijk rollidmaatschap - schermafbeelding](media/azure-pim-resource-rbac/rbac-access-review-setting-2.png)
2. Kies één rol in de lijst.

### <a name="decide-who-will-perform-the-review"></a>Bepalen wie verantwoordelijk is voor de controle
Er zijn drie opties voor het uitvoeren van een beoordeling. Kunt u de controle toewijzen aan iemand anders is voltooid, kunt u dit zelf doen of hun eigen toegang kunt bekijken voor elke gebruiker.

1. Kies een van de opties:
   
   * **Geselecteerde gebruikers**: Gebruik deze optie als u niet weet die toegang nodig heeft. Met deze optie kunt u de controle toewijzen aan een resource-eigenaar of de groepmanager te voltooien.
   * **Toegewezen (selfservicegebruiker)**: Gebruik deze optie om gebruikers hun eigen roltoewijzingen bekijken.
   
2. Ga naar **Selecteer revisoren**.
   
    ![Selecteer revisoren - schermafbeelding](media/azure-pim-resource-rbac/rbac-access-review-setting-3.png)

### <a name="start-the-review"></a>De revisie starten
Ten slotte kunt u vereisen dat gebruikers een reden voor het goedkeuren van toegang bieden. Een beschrijving van de evaluatie als u dit wilt toevoegen. Selecteer vervolgens **Start**.

Zorg ervoor dat u uw gebruikers weten dat er een wachten tot ze toegang-controleren en te laten laten [het uitvoeren van een onderzoek toegang](pim-resource-roles-perform-access-review.md).

## <a name="manage-the-access-review"></a>De controle van toegang beheren
In het dashboard PIM Azure-resources kunt u de voortgang volgen als de revisoren hun beoordelingen voltooid. Er is geen toegangsrechten zijn gewijzigd in de map tot [de controle is voltooid](pim-resource-roles-complete-access-review.md).

Totdat de controleperiode afgelopen is, kunt u gebruikers kunnen hun beoordeling voltooien herinneren of stoppen van de controle vroeg in het gedeelte van de beoordelingen toegang.

