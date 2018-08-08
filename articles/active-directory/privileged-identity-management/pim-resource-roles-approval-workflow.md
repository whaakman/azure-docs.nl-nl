---
title: Werkstroom voor goedkeuring voor Azure-resource-rollen in Privileged Identity Management | Microsoft Docs
description: Hierin wordt beschreven in het werkstroomproces goedkeuring voor Azure-resources.
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
ms.openlocfilehash: dcb306bda8ef7d93314390bd9a90327ffdd14a0e
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39619634"
---
# <a name="approval-workflow-for-azure-resource-roles-in-privileged-identity-management"></a>Werkstroom voor goedkeuring voor Azure-resource-rollen in Privileged Identity Management

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

## <a name="approve-or-deny-a-request"></a>Goedkeuren of afwijzen van een aanvraag

Als u wilt goedkeuren of weigeren van een aanvraag, moet u lid zijn van de Fiatteurlijst zijn. 

1. Selecteer in PIM, **aanvragen goedkeuren** op het tabblad in het menu links en Ga naar de aanvraag.

   ![Deelvenster 'Aanvragen goedkeuren'](media/azure-pim-resource-rbac/aadpim_rbac_approve_requests_list.png)

2. Selecteer de aanvraag, een reden opgeven wanneer ze voor de beslissing en selecteer **goedkeuren** of **weigeren**. De aanvraag is vervolgens opgelost.

   ![Geselecteerde aanvraag met gedetailleerde informatie](media/azure-pim-resource-rbac/aadpim_rbac_approve_request_approved.png)

## <a name="workflow-notifications"></a>Werkstroommeldingen

Hier vindt u enige kennis over werkstroommeldingen:

- Alle leden van de Fiatteurlijst krijgt een melding via e-mail wanneer een aanvraag voor een rol in afwachting van de beoordeling is. E-mailmeldingen bevatten een directe koppeling naar de aanvraag, waarin de fiatteur kan goedkeuren of weigeren.
- Aanvragen worden opgelost door de eerste lid van de lijst die goedkeurt of weigert. 
- Wanneer een goedkeurder op de aanvraag reageert, zijn alle leden van de Fiatteurlijst een melding van de actie. 
- Resource-beheerders worden gewaarschuwd wanneer een goedgekeurde lid actief in hun rol. 

>[!Note]
>De resourcebeheerder van een die ervan overtuigd is dat een goedgekeurde lid niet actief mag zijn, kunt de toewijzing van actieve rollen in PIM verwijderen. Hoewel resource beheerders zijn niet op de hoogte gesteld van aanvragen in behandeling, tenzij ze lid van de Fiatteurlijst zijn, kunnen ze weergeven en annuleren in behandeling zijnde aanvragen van alle gebruikers weergeven in behandeling zijnde aanvragen in PIM. 

## <a name="next-steps"></a>Volgende stappen

[PIM-instellingen toepassen op de unieke groepen van gebruikers](pim-resource-roles-custom-role-policy.md)
