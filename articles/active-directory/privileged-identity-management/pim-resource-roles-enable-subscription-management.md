---
title: Privileged Identity Management voor Azure-resources - beheer van abonnementen inschakelen | Microsoft Docs
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
ms.openlocfilehash: e164d8adaf5df63dba31bb6aa8e56f768741479c
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/04/2018
---
# <a name="enable-subscription-management-in-your-tenant"></a>Beheer van abonnementen in uw tenant inschakelen

Als globale beheerder van uw directory er standaardtoegang tot alle abonnement bronnen in uw tenant. In dit artikel worden de stappen voor uzelf toegang geven tot alle abonnementen in uw tenant. Het bevat ook een aanbevolen benadering van resterende voldoen aan eventuele beveiligingsmechanismen die uw organisatie vereist wanneer u toegang hebt ontvangen.

## <a name="who-can-enable-management-of-subscriptions-in-my-directory"></a>Wie kan beheer van abonnementen in mijn directory inschakelen?

Elke gebruiker die is toegewezen aan de rol globale beheerder moet de volgende stappen voor het beheer van abonnementen inschakelen. Nadat u het beheer van abonnementen voor uzelf hebt ingeschakeld, kunt u andere globale beheerders die resource wellicht toevoegen ook toegang. Er is geen directory-instelling waarmee u toegang hebt voor alle leden van de rol globale beheerder.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Aanmelden bij de Azure-portal met een account dat lid is van een in aanmerking komende of actieve van de rol globale beheerder. Als het account een in aanmerking komende globale beheerder is, moet u eerst de rol activeren voordat u doorgaat met de volgende stap.

## <a name="access-the-azure-active-directory-admin-center"></a>Toegang tot de Azure Active Directory-beheercentrum

Nu dat u bent aangemeld bij de Azure portal als globale beheerder, kunt u instellingen voor toegang tot Azure-abonnementen kunt bewerken. Blader naar het beheercentrum van Azure Active Directory (Azure AD) en selecteer **eigenschappen**.

![Schermopname van Azure AD-beheercentrum met eigenschappen die zijn gemarkeerd](media/azure-pim-resource-rbac/aad_properties.png)

In de lijst met eigenschappen, onder **globale beheerder Azure-abonnementen kunt beheren**, selecteer **Ja**.

![Schermafbeelding van de eigenschappenpagina met de wisselknop is ingesteld op Ja](media/azure-pim-resource-rbac/aad_properties_save.png)

Uw account is nu automatisch toegevoegd aan de rol Beheerder gebruikerstoegang voor elke bron abonnement in de tenant.

## <a name="browse-to-azure-ad-pim"></a>Blader naar Azure AD PIM

 Hier kunt gaat u naar Azure AD Privileged Identity Management (PIM). Onder **beheren**, selecteer **Azure-resources**.

![Schermopname van PIM, met Azure-resources gemarkeerd](media/azure-pim-resource-rbac/aadpim_manage_azure_resources.png)

## <a name="manage-and-discover-resources"></a>Beheren en bronnen detecteren

Als uw organisatie van Azure AD PIM al gebruikmaakt beheerders beveiligen in Azure AD, ziet u een lijst met abonnementen wanneer de blade wordt geladen.

![Schermopname van PIM, lijst met abonnementen weergegeven in de blade](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

> [!NOTE]
> Als u alle resources niet ziet, Controleer of:
>- De rol globale beheerder niet is verlopen. 
>- Uw organisatie heeft een Azure-abonnement.

![Schermopname van PIM, met lege resourcelijst](media/azure-pim-resource-rbac/aadpim_rbac_empty_resource_list.png)

## <a name="configure-assignments"></a>Toewijzingen configureren

Selecteer een abonnement uit de lijst en zelf (of een groep die u lid van bent) toewijzen als een in aanmerking komende eigenaar van de resource. 
[Meer informatie over het toewijzen van rollen](pim-resource-roles-assign-roles.md).

Herhaal dit proces voor elke resource voordat u doorgaat met de volgende stap.

## <a name="clean-up-standing-access"></a>Permanente toegang opschonen

Nu dat u in aanmerking komende toewijzingen voor de belangrijke abonnementen in uw organisatie hebt, kunt u de permanente toegang opschonen door de optie in de eigenschappen van de map uit te schakelen.

![Schermafbeelding van de eigenschappenpagina met de wisselknop is ingesteld op Nee](media/azure-pim-resource-rbac/aad_properties_no.png)

## <a name="next-steps"></a>Volgende stappen

[Resources doorzoeken](pim-resource-roles-discover-resources.md)

[Rolinstellingen configureren](pim-resource-roles-configure-role-settings.md)








