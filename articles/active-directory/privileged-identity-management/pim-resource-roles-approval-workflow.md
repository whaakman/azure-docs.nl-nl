---
title: Privileged Identity Management - werkstroom voor goedkeuring voor Azure-resourcerollen | Microsoft Docs
description: Hierin wordt beschreven hoe de werkstroom voor goedkeuring verwerken voor Azure-resources.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: c02d595d75b2d63558896054c185102ebb23cc9e
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-roles---approve"></a>Privileged Identity Management - resourcerollen - goedkeuren

Beheerders kunnen met van de goedkeuringswerkstroom in PIM voor Azure-resourcerollen, verder beveiligen of toegang tot kritieke bronnen beperken door goedkeuring om te activeren roltoewijzingen wordt vereist. Uniek is voor de functies van Azure-resource is het concept van een resource-hiërarchie. Deze hiërarchie zorgt ervoor dat de overname van roltoewijzingen van een bovenliggende resource object naar beneden naar alle onderliggende onderliggende bronnen binnen de bovenliggende container. 

Bijvoorbeeld Bob, de resourcebeheerder van een maakt gebruik van PIM Els als lid van een in aanmerking komende toewijzen aan de rol van eigenaar in het Contoso-abonnement. Met deze toewijzing is Els ook een in aanmerking komende eigenaar van alle containers van resource groep binnen de Contoso-abonnement en alle resources (zoals virtuele machines) binnen elke resourcegroep van het abonnement. Stel er zijn drie resourcegroepen in de Contoso-abonnement. Fabrikam Test, Fabrikam Dev en Fabrikam Prod. Elk van deze brongroepen bevat één virtuele machine.

PIM-instellingen zijn geconfigureerd voor elke rol van een resource en in tegenstelling tot toewijzingen deze instellingen niet worden overgenomen en uitsluitend van toepassing op de resourcerol. [Lees meer over in aanmerking komende toewijzingen en de zichtbaarheid van de resource.](pim-resource-roles-eligible-visibility.md)

In het bovenstaande voorbeeld Bob PIM gebruikt voor het vereisen van alle leden in de rol van eigenaar van de aanvraag voor goedkeuring van Contoso abonnement te activeren. Omwille van de resources die zich in de resourcegroep Fabrikam Prod Bob goedkeuring voor leden van de rol van eigenaar van deze resource is ook vereist. De eigenaar van rollen in Fabrikam Test- en Fabrikam Dev vereisen geen goedkeuring om te activeren.

Als Els aanvragen activering van de rol van haar eigenaar voor het abonnement voor Contoso een goedkeurder moet toestaan of weigeren haar aanvraag voordat ze actief in de rol. Bovendien als Alice wil [haar activering bereik](pim-resource-roles-activate-your-roles.md#just-enough-administration) aan de resourcegroep Fabrikam Prod goedkeurder moet goedkeuren of weigeren van deze aanvraag te. Echter, als Alice wil haar activering op een of beide Fabrikam-Test of Fabrikam Dev bereik, goedkeuring wordt niet vereist.

Werkstroom voor goedkeuring is mogelijk niet nodig zijn voor alle leden van een rol. Neem bijvoorbeeld een scenario waar uw organisatie huurt verschillende contract gekoppeld om te helpen bij het ontwikkelen van een toepassing die wordt uitgevoerd in een Azure-abonnement. Als de resourcebeheerder van een, u wilt dat werknemers van het in aanmerking komende toegang zonder goedkeuring nodig hebben, maar koppelt van het contract moeten goedkeuring aanvragen. Voor het configureren van de werkstroom voor goedkeuring voor alleen het contract is gekoppeld, kunt u een aangepaste beveiligingsrol maken met dezelfde machtigingen als de rol toegewezen aan werknemers, en goedkeuring vereist voor deze aangepaste rol activeren. [Meer informatie over aangepaste rollen](pim-resource-roles-custom-role-policy.md).

Volg onderstaande stappen voor het configureren van de werkstroom voor goedkeuring en opgeven die kunt goedkeuren of weigeren van aanvragen.

## <a name="require-approval-to-activate"></a>Goedkeuring vereisen om deze rol te activeren

Navigeer naar PIM in de Azure portal en selecteert u een resource in de lijst.

![](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

Selecteer in het navigatiemenu links **rolinstellingen**.

Zoek en selecteer een rol en klik op **bewerken** om instellingen te wijzigen.

![](media/azure-pim-resource-rbac/aadpim_rbac_role_settings_view_settings.png)

In de activering van de sectie het selectievakje **goedkeuring vereist voor het activeren van**.

![](media/azure-pim-resource-rbac/aadpim_rbac_settings_require_approval_checkbox.png)

## <a name="specify-approvers"></a>Goedkeurders opgeven

Klik op **fiatteurs selecteren** het selectiescherm openen.

>[!NOTE]
>U moet ten minste één gebruiker of groep bijwerken van de instelling selecteren. Er zijn geen standaard goedkeurders.

Resource-beheerders kunnen een combinatie van gebruikers en groepen toevoegen aan de lijst met goedkeurders. 

![](media/azure-pim-resource-rbac/aadpim_rbac_role_settings_select_approvers.png)

## <a name="request-approval-to-activate"></a>Goedkeuring aanvragen om te activeren

Aanvragen van goedkeuring heeft geen invloed op de procedure moet lid volgen om te activeren. [Bekijk de stappen voor het activeren van een rol](pim-resource-roles-activate-your-roles.md).

Als een lid aangevraagd activering van een rol die goedkeuring vereist en de rol niet langer vereist is, kan het lid hun aanvraag in PIM annuleren.

Als u wilt annuleren, gaat u naar PIM en selecteer 'Mijn aanvragen'. Zoek de aanvraag en klik op 'Annuleren'.

![](media/azure-pim-resource-rbac/aadpim_rbac_role_approval_request_pending.png)

## <a name="approve-or-deny-a-request"></a>Goedkeuren of weigeren van een aanvraag

Als u wilt goedkeuren of weigeren van een aanvraag moet u lid zijn van de lijst goedkeurder. In PIM, selecteert u 'Aanvragen goedkeuren' op het tabblad in het navigatiemenu links en zoek de aanvraag.

![](media/azure-pim-resource-rbac/aadpim_rbac_approve_requests_list.png)

Selecteer de aanvraag, Geef een reden voor de beslissing en selecteert u goedkeuren of weigeren, het omzetten van de aanvraag.

![](media/azure-pim-resource-rbac/aadpim_rbac_approve_request_approved.png)

## <a name="workflow-notifications"></a>Werkstroommeldingen

- Alle leden van de lijst goedkeurder worden per e-mail gewaarschuwd wanneer een aanvraag voor een rol in afwachting van hun revisie is. E-mailmeldingen bevatten een directe koppeling naar de aanvraag waarin de goedkeurder kunt goedkeuren of weigeren.
- Aanvragen worden opgelost door het eerste lid van de lijst keurt deze goed of weigert. 
- Wanneer een goedkeurder op de aanvraag reageert, worden alle leden van de lijst goedkeurder gewaarschuwd voor de actie. 
- Resource-beheerders worden gewaarschuwd wanneer een goedgekeurde lid actief in hun rol. 

>[!Note]
>In een gebeurtenis waar de resourcebeheerder van een gelooft dat het goedgekeurde lid niet actief mag zijn, kunnen ze de actieve roltoewijzing in PIM verwijderen. Hoewel beheerders van de resource niet gemeld in behandeling zijnde aanvragen tenzij ze lid van de lijst goedkeurder zijn, kunnen ze weergeven en annuleren in behandeling zijnde aanvragen van alle gebruikers door te bekijken in behandeling zijnde aanvragen in PIM. 

## <a name="next-steps"></a>Volgende stappen

[PIM-instellingen toepassen op unieke groepen van gebruikers](pim-resource-roles-custom-role-policy.md)
