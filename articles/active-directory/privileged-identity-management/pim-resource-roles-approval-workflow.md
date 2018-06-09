---
title: Werkstroom voor goedkeuring voor Azure-resourcerollen in Privileged Identity Management | Microsoft Docs
description: Hierin wordt beschreven in het werkstroomproces goedkeuring voor Azure-resources.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: protection
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: de15a02e706ec7f7b4cff0af303ea30fc87b8f34
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35233764"
---
# <a name="approval-workflow-for-azure-resource-roles-in-privileged-identity-management"></a>Werkstroom voor goedkeuring voor Azure-resourcerollen in Privileged Identity Management

Beheerders kunnen met de goedkeuringswerkstroom in Privileged Identity Management (PIM) voor Azure-resourcerollen verder beveiligen of toegang beperken tot kritieke bronnen. Dat wil zeggen dat beheerders goedkeuring om te activeren roltoewijzingen kunnen vereisen. 

Het concept van een hiërarchie van de resource is uniek voor de functies van Azure-resource. Deze hiërarchie zorgt ervoor dat de overname van roltoewijzingen van een bovenliggende resource object naar beneden naar alle onderliggende resources in de bovenliggende container. 

Bijvoorbeeld: Bob, resourcebeheerder van een PIM gebruikt Els als lid van een in aanmerking komende toewijzen aan de rol van eigenaar in het Contoso-abonnement. Met deze toewijzing is Els een in aanmerking komende eigenaar van alle containers van resource groep binnen de Contoso-abonnement. Els is ook een in aanmerking komende eigenaar van alle resources (zoals virtuele machines) binnen elke resourcegroep van het abonnement. 

Stel er zijn drie resourcegroepen in de Contoso-abonnement: Fabrikam Test, Fabrikam Dev en Fabrikam Prod. Elk van deze brongroepen bevat één virtuele machine.

PIM-instellingen zijn geconfigureerd voor elke rol van een resource. In tegenstelling tot toewijzingen, worden deze instellingen niet worden overgenomen en uitsluitend van toepassing op de resourcerol. [Lees meer over in aanmerking komende toewijzingen en de zichtbaarheid van de resource](pim-resource-roles-eligible-visibility.md).

U kunt doorgaan met het voorbeeld: Bob PIM gebruikt voor het vereisen van alle leden in de rol van eigenaar van de aanvraag voor goedkeuring van Contoso abonnement moeten worden geactiveerd. Ter bescherming van de resources in de resourcegroep Fabrikam Prod Bob is ook vereist goedkeuring voor leden van de rol van eigenaar van deze resource. De eigenaar van rollen in Fabrikam Test- en Fabrikam Dev vereisen geen goedkeuring voor activering.

Wanneer Els activering van de rol van haar eigenaar voor het Contoso-abonnement aanvraagt, moet een goedkeurder goedkeuren of haar aanvraag weigeren voordat ze actief in de rol. Als Els wil [haar activering bereik](pim-resource-roles-activate-your-roles.md#apply-just-enough-administration-practices) aan de resourcegroep Fabrikam Prod goedkeurder moet goedkeuren of weigeren van deze aanvraag te. Maar als Alice wil haar activering op een of beide Fabrikam-Test of Fabrikam Dev bereik, goedkeuring is niet vereist.

De werkstroom voor goedkeuring is mogelijk niet nodig zijn voor alle leden van een rol. Neem bijvoorbeeld een scenario waar uw organisatie verschillende contract gekoppeld huurt om te helpen bij het ontwikkelen van een toepassing die wordt uitgevoerd in een Azure-abonnement. Als de resourcebeheerder van een, u wilt dat werknemers in aanmerking komende toegang zonder goedkeuring nodig hebben, maar koppelt van het contract moeten goedkeuring aanvragen. Voor meer informatie over het configureren van werkstroom voor goedkeuring voor de koppelt van het contract kunt u een aangepaste beveiligingsrol maken met dezelfde machtigingen als de rol die is toegewezen aan werknemers. U kunt goedkeuring vereist voor deze aangepaste rol activeren. [Meer informatie over aangepaste rollen](pim-resource-roles-custom-role-policy.md).

Gebruik de volgende procedures voor het configureren van de werkstroom voor goedkeuring en die kan worden goedgekeurd of aanvragen weigeren opgeeft.

## <a name="require-approval-to-activate"></a>Goedkeuring vereisen om deze rol te activeren

1. Blader naar PIM in de Azure portal en selecteert u een resource in de lijst.

   ![Deelvenster 'Azure-resources' met een geselecteerde resource](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

2. Selecteer in het linkerdeelvenster **rolinstellingen**.

3. Zoek en selecteer een rol en selecteer vervolgens **bewerken** om instellingen te wijzigen.

   ![Knop voor de operatorrol 'Bewerken'](media/azure-pim-resource-rbac/aadpim_rbac_role_settings_view_settings.png)

4. In de **activering** sectie, selecteer de **goedkeuring vereist voor het activeren** selectievakje.

   !['Activering'-sectie van serverfunctie-instellingen](media/azure-pim-resource-rbac/aadpim_rbac_settings_require_approval_checkbox.png)

## <a name="specify-approvers"></a>Goedkeurders opgeven

Klik op **fiatteurs selecteren** openen de **gebruiker of groep selecteren** deelvenster.

>[!NOTE]
>U moet ten minste één gebruiker of groep bijwerken van de instelling selecteren. Er zijn geen standaard goedkeurders.

Resource-beheerders kunnen een combinatie van gebruikers en groepen toevoegen aan de lijst met goedkeurders. 

![Deelvenster 'Voor het selecteren van een gebruiker of groep' met een gebruiker geselecteerd](media/azure-pim-resource-rbac/aadpim_rbac_role_settings_select_approvers.png)

## <a name="request-approval-to-activate"></a>Goedkeuring aanvragen om te activeren

Goedkeuring aanvraagt, heeft geen invloed op de procedure die een lid volgen moet om te activeren. [Bekijk de stappen voor het activeren van een rol](pim-resource-roles-activate-your-roles.md).

Als een lid aangevraagd activering van een rol die goedkeuring vereist en de rol niet langer vereist is, kan het lid hun aanvraag in PIM annuleren.

Als u wilt annuleren, bladert u naar PIM en selecteer **mijn aanvragen**. Zoek de aanvraag en selecteer **annuleren**.

![Deelvenster 'Mijn aanvragen'](media/azure-pim-resource-rbac/aadpim_rbac_role_approval_request_pending.png)

## <a name="approve-or-deny-a-request"></a>Goedkeuren of weigeren van een aanvraag

Wilt goedkeuren of weigeren van een aanvraag, moet u lid zijn van de goedkeurder-lijst. 

1. Selecteer in de PIM, **aanvragen goedkeuren** op het tabblad op het menu links en zoek de aanvraag.

   ![Deelvenster 'Aanvragen goedkeuren'](media/azure-pim-resource-rbac/aadpim_rbac_approve_requests_list.png)

2. Selecteer de aanvraag, Geef een reden voor de beslissing en selecteert u **goedkeuren** of **weigeren**. De aanvraag is vervolgens opgelost.

   ![Geselecteerde aanvraag met gedetailleerde informatie](media/azure-pim-resource-rbac/aadpim_rbac_approve_request_approved.png)

## <a name="workflow-notifications"></a>Werkstroommeldingen

Hier volgen enkele feiten over werkstroommeldingen:

- Alle leden van de lijst goedkeurder worden per e-mail gewaarschuwd wanneer een aanvraag voor een rol in afwachting van hun revisie is. E-mailmeldingen bevatten een directe koppeling naar de aanvraag, waarbij de goedkeurder kunt goedkeuren of weigeren.
- Aanvragen worden opgelost door het eerste lid van de lijst die keurt deze goed of weigert. 
- Wanneer een goedkeurder op de aanvraag reageert, worden alle leden van de lijst goedkeurder gewaarschuwd voor de actie. 
- Resource-beheerders worden gewaarschuwd wanneer een goedgekeurde lid actief in hun rol. 

>[!Note]
>De resourcebeheerder van een die gelooft dat een goedgekeurde lid mag geen active kan de actieve roltoewijzing in PIM verwijderen. Hoewel beheerders van de resource niet gemeld in behandeling zijnde aanvragen tenzij ze lid van de lijst goedkeurder zijn, kunnen ze weergeven en annuleren in behandeling zijnde aanvragen van alle gebruikers in behandeling zijnde aanvragen in PIM hand. 

## <a name="next-steps"></a>Volgende stappen

[PIM-instellingen toepassen op unieke groepen van gebruikers](pim-resource-roles-custom-role-policy.md)
