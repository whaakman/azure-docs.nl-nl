---
title: Een access-revisie starten in PIM voor Azure-Resources | Microsoft Docs
description: Wordt uitgelegd hoe u een access-revisie starten in Privileged Identity Management voor Azure-Resources
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
ms.openlocfilehash: 61ed4e82e0b782b423668564dae6efb272967702
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-role---start-access-review"></a>Privileged Identity Management - Resource role - Start toegang controleren
Roltoewijzingen worden 'verouderde' wanneer gebruikers uitgebreide toegang dat ze niet meer nodig hebt. Om te reduceren het risico dat samenhangt met deze verouderde roltoewijzingen, bevoorrechte rol beheerders regelmatig wordt aangeraden de functies die gebruikers hebben gekregen. Dit document worden de stappen voor het starten van een onderzoek toegang in Privileged Identity Management (PIM) voor Azure-resources.

Navigeer via de PIM-toepassing hoofdpagina naar:

* **Toegang tot beoordelingen** > **toevoegen**

![](media/azure-pim-resource-rbac/rbac-access-review-home.png)

Wanneer u klikt op in de **toevoegen** knop, de **maken een bekijken toegang** blade wordt weergegeven. Op deze blade en gaat u naar de revisie configureren met een naam en een tijdslimiet, kiest u een rol om te controleren en beslissen wie verantwoordelijk is voor de controle.

![](media/azure-pim-resource-rbac/rbac-create-access-review.png)

### <a name="configure-the-review"></a>De controle configureren
Voor het maken van een onderzoek toegang, moet u deze de naam en stel een begin- en datum.

![Configureren van de evaluatie - schermafbeelding](media/azure-pim-resource-rbac/rbac-access-review-setting-1.png)

Controleer de lengte van de evaluatie lang genoeg is voor gebruikers om deze te voltooien. Als u klaar voor de einddatum vallen bent, kunt u de controle vroeg altijd stoppen.

### <a name="choose-a-role-to-review"></a>Kies een rol te bekijken
Iedere evaluatie is gericht op slechts één rol. Tenzij u de controle van toegang vanuit een specifieke rolblade hebt gestart, moet u nu een rol kiezen.

1. Navigeer naar **lidmaatschap van de gebruikersrol controleren**
   
    ![Bekijk rollidmaatschap - schermafbeelding](media/azure-pim-resource-rbac/rbac-access-review-setting-2.png)
2. Kies één rol in de lijst.

### <a name="decide-who-will-perform-the-review"></a>Bepalen wie verantwoordelijk is voor de controle
Er zijn drie opties voor het uitvoeren van een beoordeling. Kunt u de controle toewijzen aan iemand anders is voltooid, kunt u dit zelf doen of u kunt elke gebruiker bekijken van hun eigen toegang hebben.

1. Kies een van de opties:
   
   * **Geselecteerde gebruikers**: Gebruik deze optie als u niet weet die toegang nodig heeft. Met deze optie kunt u de controle toewijzen aan een resource-eigenaar of de groepmanager te voltooien.
   * **Toegewezen (selfservicegebruiker)**: Gebruik deze optie om gebruikers hun eigen roltoewijzingen bekijken.
   
2. Navigeer naar **revisoren selecteren**
   
    ![Selecteer revisoren - schermafbeelding](media/azure-pim-resource-rbac/rbac-access-review-setting-3.png)

### <a name="start-the-review"></a>De revisie starten
Tot slot hebt u de optie om te vereisen dat gebruikers een reden opgeven als ze hun toegang wordt goedgekeurd. Indien gewenst een beschrijving van de evaluatie toevoegen en selecteer **Start**.

Zorg ervoor dat u uw gebruikers weten dat er een wachten tot ze toegang-controleren en te laten laten [het uitvoeren van een onderzoek toegang](pim-resource-roles-perform-access-review.md).

## <a name="manage-the-access-review"></a>De controle van toegang beheren
U kunt de voortgang volgen als de revisoren hun beoordelingen in het dashboard PIM Azure-Resources in de sectie toegang beoordelingen voltooid. Er is geen toegangsrechten worden gewijzigd in de map tot [de controle is voltooid](pim-resource-roles-complete-access-review.md).

Totdat de controleperiode afgelopen is, kunt u gebruikers kunnen hun beoordeling voltooien herinneren of stoppen van de controle vroeg in het gedeelte van de beoordelingen toegang.

