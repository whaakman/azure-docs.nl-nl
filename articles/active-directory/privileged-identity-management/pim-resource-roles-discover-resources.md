---
title: Ontdek Azure-resources te beheren in PIM - Azure Active Directory | Microsoft Docs
description: Informatie over het detecteren van Azure-resources te beheren in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/09/2019
ms.author: rolyon
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1b9ca4862f129b2da23a1d1ad8bb0b1bd0a5078f
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476275"
---
# <a name="discover-azure-resources-to-manage-in-pim"></a>Azure-resources te beheren in PIM detecteren

Met behulp van Azure Active Directory (Azure AD) Privileged Identity Management (PIM), kunt u de beveiliging van uw Azure-resources verbeteren. Dit is handig voor organisaties die al gebruikmaken van PIM ter bescherming van Azure AD-rollen, en management groep en abonnement eigenaren die op zoek bent naar de productieresources beveiligen.

Wanneer u eerst ingesteld PIM voor Azure-resources, moet u om te detecteren en selecteer de resources met PIM moeten worden beveiligd. Er is geen limiet voor het aantal resources die u met PIM kunt beheren. We raden echter aan beginnen met uw meest kritieke (productie)-resources.

## <a name="discover-resources"></a>Detecteren van bronnen

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. Open **Azure AD Privileged Identity Management**.

1. Klik op **Azure-resources**.

    Als dit de eerste keer met PIM voor Azure-resources, ziet u een deelvenster van de resources detecteren.

    ![Resourcesdeelvenster detecteren met geen bronnen die worden vermeld voor de eerste ervaring van de tijd](./media/pim-resource-roles-discover-resources/discover-resources-first-run.png)

    Als Azure-resources in PIM is al door een andere resource of directory-beheerder in uw organisatie worden beheerd, ziet u een lijst van de resources die momenteel worden beheerd.

    ![Detecteren van bronnen deelvenster aanbieding bronnen die momenteel worden beheerd](./media/pim-resource-roles-discover-resources/discover-resources.png)

1. Klik op **bronnen detecteren** om te starten van de discovery-ervaring.

    ![Detectie-deelvenster resources die kunnen worden beheerd, zoals abonnementen en beheergroepen weergeven](./media/pim-resource-roles-discover-resources/discovery-pane.png)

1. In het deelvenster detectie gebruiken **filteren op resourcestatus** en **Selecteer resourcetype** voor het filteren van het beheer van groepen of abonnementen die u hebt toestemming om te schrijven. Het gemakkelijkst waarschijnlijk beginnen met **alle** in eerste instantie.

    U kunt alleen Zoek en selecteer de management-groep of abonnement resources te beheren met PIM. Wanneer u een beheergroep of een abonnement in PIM beheert, kunt u ook de onderliggende resources te beheren.

1. Voeg een vinkje naast eventuele niet-beheerde resources die u wilt beheren.

1. Klik op **resource beheren** om te beginnen met het beheren van de geselecteerde resources.

    > [!NOTE]
    > Zodra een beheergroep of het abonnement is ingesteld op beheerde, kan deze niet-beheerde niet. Dit voorkomt dat een andere resourcebeheerder PIM-instellingen worden verwijderd.

    ![Detectie-deelvenster met de geselecteerde resource en de beheren-resource-optie is gemarkeerd](./media/pim-resource-roles-discover-resources/discovery-manage-resource.png)

1. Als u een bericht om te bevestigen van de onboarding van de geselecteerde resource voor het beheer van ziet, klikt u op **Ja**.

    ![Bericht bevestigen voor Onboarding van de geselecteerde resources voor het beheer van](./media/pim-resource-roles-discover-resources/discovery-manage-resource-message.png)

## <a name="next-steps"></a>Volgende stappen

- [Azure-resource rolinstellingen in PIM configureren](pim-resource-roles-configure-role-settings.md)
- [Azure-resource-rollen in PIM toewijzen](pim-resource-roles-assign-roles.md)
