---
title: Azure-resources te beheren in PIM detecteren | Microsoft Docs
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
ms.component: pim
ms.date: 08/30/2018
ms.author: rolyon
ms.openlocfilehash: d9a6ab49d619e487eee6fb13abe128cfc167b560
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43306686"
---
# <a name="discover-azure-resources-to-manage-in-pim"></a>Azure-resources te beheren in PIM detecteren

Met behulp van Azure AD Privileged Identity Management (PIM), kunt u de beveiliging van uw Azure-resources verbeteren. Dit is handig voor organisaties die al gebruikmaken van PIM ter bescherming van Azure AD-maprollen, en management groep en abonnement eigenaren die op zoek bent naar de productieresources beveiligen.

Wanneer u eerst ingesteld PIM voor Azure-resources, moet u om te detecteren en selecteer de resources met PIM moeten worden beveiligd. Er is geen limiet voor het aantal resources die u met PIM kunt beheren. We raden echter aan beginnen met uw meest kritieke (productie)-resources.

## <a name="discover-resources"></a>Services detecteren

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. Open **Azure AD Privileged Identity Management**.

1. Klik op **Azure-resources**.

    Als dit de eerste keer met PIM voor Azure-resources, ziet u een deelvenster van de resources detecteren.

    ![Bronnen - eerst detecteren](./media/pim-resource-roles-discover-resources/discover-resources-first-run.png)

    Als Azure-resources in PIM is al door een andere resource of directory-beheerder in uw organisatie worden beheerd, ziet u een lijst van de resources die momenteel worden beheerd.

    ![Resourcesdeelvenster detecteren](./media/pim-resource-roles-discover-resources/discover-resources.png)

1. Klik op **bronnen detecteren** om te starten van de discovery-ervaring.

    ![Detectie van deelvenster](./media/pim-resource-roles-discover-resources/discovery-pane.png)

1. In het deelvenster detectie gebruiken **filteren op resourcestatus** en **Selecteer resourcetype** voor het filteren van het beheer van groepen of abonnementen die u hebt toestemming om te schrijven. Het gemakkelijkst waarschijnlijk beginnen met **alle** in eerste instantie.

    U kunt alleen Zoek en selecteer de management-groep of abonnement resources te beheren met PIM. Wanneer u een beheergroep of een abonnement in PIM beheert, kunt u ook de onderliggende resources te beheren.

1. Voeg een vinkje naast eventuele niet-beheerde resources die u wilt beheren.

    > [!NOTE]
    > Zodra een beheergroep of het abonnement is ingesteld op beheerde, kan deze niet-beheerde niet. Dit voorkomt dat een andere resourcebeheerder PIM-instellingen worden verwijderd.

    ![Detectie - resource beheren](./media/pim-resource-roles-discover-resources/discovery-manage-resource.png)

1. Klik op **resource beheren** om te beginnen met het beheren van de geselecteerde resources.

## <a name="next-steps"></a>Volgende stappen

- [Azure-resource rolinstellingen in PIM configureren](pim-resource-roles-configure-role-settings.md)
- [Azure-resource-rollen in PIM toewijzen](pim-resource-roles-assign-roles.md)
