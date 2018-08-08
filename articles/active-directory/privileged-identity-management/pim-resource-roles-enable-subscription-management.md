---
title: Privileged Identity Management voor Azure-resources - abonnementsbeheer inschakelen | Microsoft Docs
description: Informatie over hoe globale beheerders abonnementen in de tenant kunt beheren.
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
ms.date: 03/27/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: d7a9c0090d3bfaaf1161b6255c4c0b659c77d692
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39620277"
---
# <a name="enable-subscription-management-in-your-tenant"></a>Beheer van abonnementen in uw tenant inschakelen

Als een globale beheerder van uw directory, kan u niet standaardtoegang tot alle resources van het abonnement hebt in uw tenant. In dit artikel bevat een overzicht van de stappen voor het zelf toegang geven tot alle abonnementen in uw tenant. Het biedt ook een aanbevolen aanpak voor het resterende compatibel zijn met alle beveiligingsmaatregelen die uw organisatie vereist wanneer u toegang hebt ontvangen.

## <a name="who-can-enable-management-of-subscriptions-in-my-directory"></a>Wie kan beheer van abonnementen in mijn directory inschakelen?

Elke gebruiker die is toegewezen aan de rol globale beheerder moet de volgende stappen om in te schakelen van beheer van abonnementen. Nadat u het beheer van abonnementen zelf hebt ingeschakeld, kunt u andere hoofdbeheerders mogelijk resource toevoegen ook toegang. Er is geen directory-instelling waarmee u toegang hebt voor alle leden van de rol globale beheerder.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Aanmelden bij de Azure-portal met een account dat een in aanmerking komende of actief lid is van de rol globale beheerder. Als het account een in aanmerking komende globale beheerder is, moet u eerst de rol activeren voordat u doorgaat met de volgende stap.

## <a name="access-the-azure-active-directory-admin-center"></a>Toegang tot het Azure Active Directory-beheercentrum

Nu dat u bent aangemeld bij Azure portal als globale beheerder, kunt u de instellingen die toegang tot Azure-abonnementen bieden kunt bewerken. Blader naar het beheercentrum van Azure Active Directory (Azure AD) en selecteer **eigenschappen**.

![Schermafbeelding van de Azure AD-beheercentrum, met eigenschappen die zijn gemarkeerd](media/azure-pim-resource-rbac/aad_properties.png)

In de lijst met eigenschappen, onder **globale beheerder kan Azure-abonnementen beheren**, selecteer **Ja**.

![Schermafbeelding van de eigenschappenpagina met in-/ uitschakelen ingesteld op Ja](media/azure-pim-resource-rbac/aad_properties_save.png)

Uw account is nu automatisch toegevoegd aan de rol Beheerder gebruikerstoegang voor elke abonnementsresource in de tenant.

## <a name="browse-to-azure-ad-pim"></a>Blader naar Azure AD PIM

 Hier gaat u naar Azure AD Privileged Identity Management (PIM). Onder **beheren**, selecteer **Azure-resources**.

![Schermafbeelding van PIM, met Azure-resources gemarkeerd](media/azure-pim-resource-rbac/aadpim_manage_azure_resources.png)

## <a name="manage-and-discover-resources"></a>Beheren en bronnen detecteren

Als uw organisatie al Azure AD PIM met beheerders beveiligen in Azure AD, kunt u een lijst met abonnementen zien wanneer de blade wordt geladen.

![Schermafbeelding van PIM, met de lijst met abonnementen weergegeven in de blade](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

> [!NOTE]
> Als u alle resources die niet ziet, controleert u dat:
>- De rol globale beheerder is niet verlopen. 
>- Uw organisatie heeft een Azure-abonnement.

![Schermafbeelding van PIM, met lege resourcelijst](media/azure-pim-resource-rbac/aadpim_rbac_empty_resource_list.png)

## <a name="configure-assignments"></a>Toewijzingen configureren

Selecteer een abonnement in de lijst en uzelf (of een groep die u lid van bent) toewijzen als eigenaar van een in aanmerking komende van de resource. 
[Meer informatie over het toewijzen van rollen](pim-resource-roles-assign-roles.md).

Herhaal dit proces voor elke resource voordat u doorgaat met de volgende stap.

## <a name="clean-up-standing-access"></a>Opschonen van de vaste toegang heeft

Nu dat u in aanmerking komende toewijzingen voor de belangrijke abonnementen in uw organisatie hebt, kunt u permanente toegang opschonen door de optie in de eigenschappen van de map uit te schakelen.

![Schermafbeelding van de eigenschappenpagina met in-/ uitschakelen ingesteld op Nee](media/azure-pim-resource-rbac/aad_properties_no.png)

## <a name="next-steps"></a>Volgende stappen

[Resources doorzoeken](pim-resource-roles-discover-resources.md)

[Rolinstellingen configureren](pim-resource-roles-configure-role-settings.md)








