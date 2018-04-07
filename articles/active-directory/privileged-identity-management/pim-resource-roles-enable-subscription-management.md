---
title: Privileged Identity Management voor Azure-Resources - beheer van abonnementen inschakelen | Microsoft Docs
description: Meer informatie over hoe globale beheerders abonnementen in de tenant kunt beheren.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/27/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 4c6ae3da34fe5157314b8ea422591f7ecbd2a667
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="enable-subscription-management"></a>Abonnementbeheer inschakelen

Als globale beheerder van uw directory, mag u geen toegang tot alle abonnementresources hebben in uw tenant. In dit artikel wordt een overzicht van de stappen voor het zelf toegang geven tot alle abonnementen in uw tenant en een aanbevolen benadering van resterende voldoen aan eventuele beveiligingsmechanismen die uw organisatie vereist na de ontvangst van toegang.

## <a name="who-can-enable-management-of-subscriptions-in-my-directory"></a>Wie kan beheer van abonnementen in mijn directory inschakelen?

Elke gebruiker die is toegewezen aan de rol globale beheerder moet de volgende stappen voor het beheer van abonnementen inschakelen. Wanneer u het beheer van abonnementen voor uzelf hebt ingeschakeld, kunt u andere globale beheerders die ook toegang tot bedrijfsbronnen wellicht kunt toevoegen. Er is geen directory-instelling waarmee u toegang hebt voor alle leden van de rol globale beheerder.

## <a name="log-on-to-the-azure-portal"></a>Meld u aan bij de Azure portal

Beginnen met het aanmelden bij de Azure-portal met een account dat lid is van een in aanmerking komende of actieve van de rol globale beheerder. Als het account een in aanmerking komende globale beheerder die u eerst de rol activeren is moet voordat u doorgaat met de volgende stap.

## <a name="access-the-azure-ad-admin-center"></a>Toegang tot de Azure AD-beheercentrum

Nu dat u bent aangemeld bij de Azure-portal als globale beheerder kunt u instellingen waarmee u toegang tot Azure-abonnementen kunt bewerken. Navigeer naar de Azure AD-beheercentrum, zoek en selecteer het tabblad Eigenschappen in de linkernavigatiebalk.

![](media/azure-pim-resource-rbac/aad_properties.png)

Stel de optie 'globale beheerder kunt Azure-abonnementen beheren' op 'Ja' in de lijst met eigenschappen.

![](media/azure-pim-resource-rbac/aad_properties_save.png)

## <a name="navigate-to-azure-ad-pim"></a>Navigeer naar Azure AD PIM

Met deze optie is ingeschakeld, wordt uw account automatisch toegevoegd aan de rol 'Beheerder voor gebruikerstoegang' voor elke bron abonnement in de tenant. Hier kunt navigeren naar Azure AD PIM en Azure-resources onder de sectie beheren van de Linkernavigatie selecteren.

![](media/azure-pim-resource-rbac/aadpim_manage_azure_resources.png)

## <a name="manage-and-discover-resources"></a>Beheren en bronnen detecteren

Als uw organisatie al van Azure AD PIM gebruikmaakt beveiligen beheerders in Azure Active Directory ziet u een lijst met abonnementen wanneer de blade wordt geladen.

![](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

> [!NOTE]
> Als u alle resources niet ziet, controleert:
>- De rol globale beheerder niet is verlopen 
>- Uw organisatie beschikt over een Azure-abonnement

![](media/azure-pim-resource-rbac/aadpim_rbac_empty_resource_list.png)

## <a name="configure-assignments"></a>Toewijzingen configureren

Selecteer een abonnement uit de lijst en zelf (of een groep die u lid van bent) toewijzen als eigenaar van de bron van een in aanmerking komen. 
[Meer informatie over het toewijzen van rollen](pim-resource-roles-assign-roles.md).

Herhaal dit proces voor elke resource voordat u doorgaat met de volgende stap.

## <a name="clean-up-standing-access"></a>Permanente toegang opschonen

Nu dat u in aanmerking komende toewijzingen voor de belangrijke abonnementen in uw organisatie hebt, kunt u de permanente toegang opruimen door de optie in de eigenschappen van de map uit te schakelen:

![](media/azure-pim-resource-rbac/aad_properties_no.png)

## <a name="next-steps"></a>Volgende stappen

[Detecteren van bronnen](pim-resource-roles-discover-resources.md)

[Functie-instellingen configureren](pim-resource-roles-configure-role-settings.md)








