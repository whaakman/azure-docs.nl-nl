---
title: Azure-resource rolinstellingen in PIM configureren | Microsoft Docs
description: Informatie over het configureren van instellingen voor de sitesysteemrol Azure-resource in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 901eb5ef43ddb2840ed7a3d83fc08f2f05849461
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189732"
---
# <a name="configure-azure-resource-role-settings-in-pim"></a>Azure-resource rolinstellingen in PIM configureren

Wanneer u de serverfunctie-instellingen configureert, definieert u de standaardinstellingen die worden toegepast op de toewijzingen in de omgeving voor Privileged Identity Management (PIM). Voor het definiëren van deze instellingen voor uw resource, selecteer de **rolinstellingen** tabblad in het linkerdeelvenster. U kunt ook de rol van de knop in de actiebalk (in een rol) selecteren om de huidige opties weer te geven.

## <a name="overview"></a>Overzicht

Beheerders kunnen met de werkstroom voor goedkeuring in Privileged Identity Management (PIM) voor Azure-resourcerollen, verder beschermen of beperken van toegang tot kritieke bronnen. Dat wil zeggen, beheerders kunnen vereisen dat goedkeuring roltoewijzingen te activeren. 

Het concept van een hiërarchie van de resource is uniek voor Azure-resourcerollen. Deze hiërarchie kunt de overname van roltoewijzingen van een bovenliggende resource-object naar beneden naar alle onderliggende resources in de bovenliggende container. 

Bijvoorbeeld: Bob, de resourcebeheerder van een, PIM gebruikt voor het toewijzen van Alice als een in aanmerking komend lid aan de rol eigenaar in het Contoso-abonnement. Met deze toewijzing is Els een in aanmerking komende eigenaar van alle resource group-containers in het Contoso-abonnement. Els wordt ook de eigenaar van een in aanmerking komende van alle resources (zoals virtuele machines) in elke resourcegroep van het abonnement. 

Stel dat er zijn drie resourcegroepen in het Contoso-abonnement: Fabrikam Test Fabrikam Dev en Fabrikam Prod. Elk van deze resourcegroepen bevat één virtuele machine.

PIM-instellingen worden geconfigureerd voor elke rol van een resource. In tegenstelling tot toewijzingen, worden deze instellingen worden niet overgenomen en zijn alleen van toepassing op de functie van. [Meer informatie over in aanmerking komende toewijzingen en zichtbaarheid van de resource](pim-resource-roles-eligible-visibility.md).

U doorgaat met het voorbeeld: Bob PIM gebruikt om te vereisen dat alle leden in de rol van eigenaar van het goedkeuren van Contoso-abonnement worden geactiveerd. Ter bescherming van de resources in de resourcegroep van Fabrikam Prod, moet Bob ook worden goedgekeurd voor leden van de rol van eigenaar van deze resource. De rollen eigenaar in Fabrikam Test- en Fabrikam Dev vereisen geen goedkeuring voor activering.

Wanneer Els activering van haar eigenaarsrol voor de Contoso-abonnement aanvraagt, moet de fiatteur goedkeuren of haar aanvraag weigeren voordat ze actief in de rol. Als Alice wil [haar activering scope](pim-resource-roles-activate-your-roles.md#apply-just-enough-administration-practices) naar de resourcegroep Fabrikam Prod fiatteur moet goedkeuren of weigeren van deze aanvraag te. Maar als Alice wil het bereik van haar activering op een van beide of beide Fabrikam-Test of Fabrikam Dev, goedkeuring is niet vereist.

De werkstroom voor goedkeuring is mogelijk niet nodig zijn voor alle leden van een rol. U hebt een scenario waarbij uw organisatie verschillende contract wordt om te helpen bij de ontwikkeling van een toepassing die wordt uitgevoerd in een Azure-abonnement ingehuurd. Als de resourcebeheerder van een, u wilt dat werknemers in aanmerking komende toegang hebben zonder goedkeuring is vereist, maar koppelt van het contract moeten goedkeuring aanvragen. Werkstroom voor goedkeuring configureren voor de koppelt van het contract, kunt u een aangepaste rol maken met dezelfde machtigingen als de rol die is toegewezen aan werknemers. U kunt vereisen dat goedkeuring om deze aangepaste rol te activeren. [Meer informatie over aangepaste rollen](pim-resource-roles-custom-role-policy.md).

Gebruik de volgende procedures voor het configureren van de werkstroom voor goedkeuring en opgeven wie kan goedkeuren of weigeren.

## <a name="require-approval-to-activate"></a>Goedkeuring vereisen om deze rol te activeren

1. Blader naar PIM in Azure portal en selecteer een resource in de lijst.

   ![Deelvenster 'Azure-resources' met een geselecteerde bron](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

2. Selecteer in het linkerdeelvenster **rolinstellingen**.

3. Zoek en selecteer een rol en selecteer vervolgens **bewerken** om instellingen te wijzigen.

   ![Knop 'Bewerken' voor de rol van operator](media/azure-pim-resource-rbac/aadpim_rbac_role_settings_view_settings.png)

4. In de **activering** sectie, selecteer de **goedkeuring vereisen voor het activeren** selectievakje.

   ![De sectie '-activering van serverfunctie-instellingen](media/azure-pim-resource-rbac/aadpim_rbac_settings_require_approval_checkbox.png)

## <a name="specify-approvers"></a>Fiatteurs opgeven

Klik op **fiatteurs selecteren** openen de **selecteert u een gebruiker of groep** deelvenster.

>[!NOTE]
>U moet ten minste één gebruiker of groep om bij te werken van de instelling selecteren. Er zijn geen Standaard fiatteurs.

Resource-beheerders kunnen een combinatie van gebruikers en groepen toevoegen aan de lijst met goedkeurders. 

![Deelvenster 'Voor het selecteren van een gebruiker of groep' met de gebruiker heeft geselecteerd](media/azure-pim-resource-rbac/aadpim_rbac_role_settings_select_approvers.png)

## <a name="request-approval-to-activate"></a>Goedkeuring aanvragen om te activeren

Aanvragen van goedkeuring heeft geen invloed op de procedure die een lid volgen moet om te activeren. [Bekijk de stappen voor het activeren van een rol](pim-resource-roles-activate-your-roles.md).

Als lid van een activering van een rol waarvoor goedkeuring aangevraagd en de rol niet langer vereist is, kan het lid de aanvraag in PIM annuleren.

Als u wilt annuleren, bladert u naar PIM en selecteer **mijn aanvragen**. Zoek de aanvraag en selecteer **annuleren**.

![Deelvenster "Mijn aanvragen"](media/azure-pim-resource-rbac/aadpim_rbac_role_approval_request_pending.png)

## <a name="next-steps"></a>Volgende stappen

- [Meervoudige verificatie vereisen voor Azure-resource-rollen in PIM](pim-resource-roles-require-mfa.md)
- [Beveiligingswaarschuwingen voor Azure resource-rollen in PIM configureren](pim-resource-roles-configure-alerts.md)
