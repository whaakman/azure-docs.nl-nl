---
title: In aanmerking komende toewijzingen en de zichtbaarheid van resources in PIM - Azure | Microsoft Docs
description: Beschrijft hoe leden als in aanmerking voor Azure-resource-rollen in Azure AD Privileged Identity Management (PIM) toewijzen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 59d51ba8edadd1fd71255271623b144cab94fc97
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43344280"
---
# <a name="eligible-assignments-and-resource-visibility-in-pim"></a>In aanmerking komende toewijzingen en de zichtbaarheid van resources in PIM

Privileged Identity Management (PIM) voor Azure-resourcerollen biedt verbeterde beveiliging voor organisaties die essentiële Azure-resources hebben. Resource-beheerders kunnen PIM gebruiken voor het toewijzen van leden als in aanmerking komen voor de resource-rollen. Meer informatie over de van verschillende toewijzingstypen en de toewijzing voor de Azure-resource in de volgende secties. 

## <a name="assignment-types"></a>Toewijzingstypen

PIM voor Azure-resources biedt twee afzonderlijke toewijzingstypen:

- In aanmerking komend
- Actief

In aanmerking komende toewijzingen moeten het lid van de rol van een actie voor het gebruik van de rol uit te voeren. Acties advies inwinnen bij een controle van de multi-factor authentication slaagt, een zakelijke rechtvaardiging te geven of aanvragen van goedkeuring van fiatteurs.

Directe toewijzingen nodig geen voor het lid aan een actie voor het gebruik van de rol wordt uitgevoerd. Leden toegewezen als actief hebben de bevoegdheden die zijn toegewezen aan de rol te allen tijde.

## <a name="assignment-duration"></a>Duur van de toewijzing

Resource-beheerders kunnen kiezen uit twee opties voor elk toewijzingstype wanneer ze PIM-instellingen voor een functie configureert. Deze opties worden de standaard maximale duur voor wanneer een lid is toegewezen aan de rollen in PIM. 

Een beheerder kunt kiezen uit een van deze toewijzingstypen:

- Permanent in aanmerking komende toewijzing toestaan
- Permanente actieve toewijzing toestaan

Of een beheerder kan een van deze toewijzingstypen kiezen:

- In aanmerking komende toewijzingen na verlopen
- Actieve toewijzingen laten verlopen na

Als de resourcebeheerder van een kiest **permanent in aanmerking komende toewijzing toestaan** of **permanente actieve toewijzing toestaan**, alle beheerders die leden aan de resource toewijzen permanent kunnen toewijzen te beheren.

Als de resourcebeheerder van een kiest **verlopen van in aanmerking komende toewijzingen na** of **actieve toewijzingen na verlopen**, beheerder van de resource beheert de levenscyclus van de toewijzing door te vereisen dat alle toewijzingen van hebben een opgegeven begin- en datum.

> [!NOTE] 
> Alle toewijzingen van met een opgegeven datum kunnen worden vernieuwd door beheerders van de resource. Bovendien leden Self-service aanvragen om te kunnen initiëren [verlengen of vernieuwen toewijzingen](pim-resource-roles-renew-extend.md).


## <a name="assignment-states"></a>Toewijzing van statussen

PIM voor Azure-resources heeft twee verschillende toewijzing statussen die worden weergegeven op de **actieve rollen** tabblad de **mijn rollen**, **rollen**, en **leden**weergaven van PIM. Deze statussen zijn:

- Toegewezen
- Geactiveerd

Wanneer u een lidmaatschap dat wordt vermeld in bekijkt **actieve rollen**, kunt u de waarde in de **status** kolom onderscheid maken tussen de gebruikers die zijn **toegewezen** als actief en gebruikers die **geactiveerd** een in aanmerking komende toewijzing en zijn nu actief.

## <a name="azure-resource-role-approval-workflow"></a>Werkstroom voor goedkeuring van Azure-resource-rollen

Beheerders kunnen met de werkstroom voor goedkeuring in PIM voor Azure-resourcerollen, verder beschermen of beperken van toegang tot kritieke bronnen. Dat wil zeggen, beheerders kunnen vereisen dat goedkeuring roltoewijzingen te activeren.

Het concept van een hiërarchie van de resource is uniek voor Azure-resourcerollen. Deze hiërarchie kunt de overname van roltoewijzingen van een bovenliggende resource-object naar beneden naar alle onderliggende resources in de bovenliggende container. 

Bijvoorbeeld: Bob, de resourcebeheerder van een, PIM gebruikt voor het toewijzen van Alice als een in aanmerking komend lid aan de rol eigenaar in het Contoso-abonnement. Met deze toewijzing is Els een in aanmerking komende eigenaar van alle resource group-containers in het Contoso-abonnement. Els wordt ook de eigenaar van een in aanmerking komende van alle resources (zoals virtuele machines) in elke resourcegroep van het abonnement.

Stel dat er zijn drie resourcegroepen in het Contoso-abonnement: Fabrikam Test Fabrikam Dev en Fabrikam Prod. Elk van deze resourcegroepen bevat één virtuele machine.

PIM-instellingen worden geconfigureerd voor elke rol van een resource. In tegenstelling tot toewijzingen, worden deze instellingen worden niet overgenomen en zijn alleen van toepassing op de functie van.

U doorgaat met het voorbeeld: Bob PIM gebruikt om te vereisen dat alle leden in de rol van eigenaar van het goedkeuren van Contoso-abonnement worden geactiveerd. Ter bescherming van de resources in de resourcegroep van Fabrikam Prod, moet Bob ook worden goedgekeurd voor leden van de rol van eigenaar van deze resource. De rollen eigenaar in Fabrikam Test- en Fabrikam Dev vereisen geen goedkeuring voor activering.

Wanneer Els activering van haar eigenaarsrol voor de Contoso-abonnement aanvraagt, moet de fiatteur goedkeuren of haar aanvraag weigeren voordat ze actief in de rol. Als Alice wil [haar activering scope](pim-resource-roles-activate-your-roles.md#apply-just-enough-administration-practices) naar de resourcegroep Fabrikam Prod fiatteur moet goedkeuren of weigeren van deze aanvraag te. Maar als Alice wil het bereik van haar activering op een van beide of beide Fabrikam-Test of Fabrikam Dev, goedkeuring is niet vereist.

De werkstroom voor goedkeuring is mogelijk niet nodig zijn voor alle leden van een rol. U hebt een scenario waarbij uw organisatie verschillende contract wordt om te helpen bij de ontwikkeling van een toepassing die wordt uitgevoerd in een Azure-abonnement ingehuurd. Als de resourcebeheerder van een, u wilt dat werknemers in aanmerking komende toegang hebben zonder goedkeuring is vereist, maar koppelt van het contract moeten goedkeuring aanvragen. Werkstroom voor goedkeuring configureren voor de koppelt van het contract, kunt u een aangepaste rol maken met dezelfde machtigingen als de rol die is toegewezen aan werknemers. U kunt vereisen dat goedkeuring om deze aangepaste rol te activeren. [Meer informatie over aangepaste rollen](pim-resource-roles-custom-role-policy.md).

## <a name="next-steps"></a>Volgende stappen

- [Azure-resource-rollen in PIM toewijzen](pim-resource-roles-assign-roles.md)
