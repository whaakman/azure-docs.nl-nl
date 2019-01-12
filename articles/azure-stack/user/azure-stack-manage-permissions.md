---
title: Beheren van machtigingen voor resources per gebruiker in Azure Stack | Microsoft Docs
description: Informatie over het beheren van RBAC-machtigingen als een servicebeheerder of tenant.
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.assetid: cccac19a-e1bf-4e36-8ac8-2228e8487646
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2018
ms.author: patricka
ms.reviewer: unknown
ms.openlocfilehash: 1e2846252059efb188cfb4c0305b447857d151f4
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2019
ms.locfileid: "54244664"
---
# <a name="manage-access-to-resources-with-azure-stack-role-based-access-control"></a>Toegang tot resources met Azure Stack Role-Based toegangsbeheer beheren

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Azure Stack biedt ondersteuning voor op rollen gebaseerd toegangsbeheer (RBAC), dezelfde [beveiligingsmodel voor toegangsbeheer](https://docs.microsoft.com/azure/role-based-access-control/overview) die gebruikmaakt van Microsoft Azure. U kunt RBAC gebruiken voor het beheren van de gebruiker, groep of toepassing toegang heeft tot de abonnementen, resources en services.

## <a name="basics-of-access-management"></a>Basisprincipes van beheer van toegang

Op rollen gebaseerd toegangsbeheer biedt uiterst gedetailleerd toegangsbeheer die u gebruiken kunt voor het beveiligen van uw omgeving. U geeft gebruikers de exacte machtigingen die ze nodig hebben door toe te wijzen een RBAC-rol met een bepaald bereik. Het bereik van de roltoewijzing kan een abonnement, een resourcegroep of één resource zijn. Lees de [toegangsbeheer op basis van rollen in Azure portal](https://docs.microsoft.com/azure/role-based-access-control/overview) artikel voor meer gedetailleerde informatie over het toegangsbeheer van.

### <a name="built-in-roles"></a>Ingebouwde rollen

Azure Stack heeft drie fundamentele rollen die u op alle resourcetypen toepassen kunt:

* **Eigenaar** kan alles beheren, inclusief toegang tot bronnen.
* **Inzender** kan alles beheren behalve toegang tot bronnen.
* **Lezer** kan alles weergeven, maar geen wijzigingen kunt aanbrengen.

### <a name="resource-hierarchy-and-inheritance"></a>Resource-hiërarchie en overname

Azure Stack heeft de volgende resource-hiërarchie:

* Elk abonnement behoort tot een map.
* Elke resourcegroep behoort tot één abonnement.
* Elke resource behoort tot één resourcegroep.

Toegang die u aan een bovenliggend bereik toewijst wordt overgenomen op onderliggende bereiken. Bijvoorbeeld:

* U kunt de rol van lezer toewijzen aan een Azure AD-groep op het abonnementsbereik. De leden van die groep kunnen elke resourcegroep en de resource weergeven in het abonnement.
* U toewijzen de rol van Inzender aan een toepassing op het groepsbereik van de resource. De toepassing kunt resources van alle typen in die resourcegroep, maar geen andere resourcegroepen in het abonnement beheren.

### <a name="assigning-roles"></a>Toewijzen van rollen

U kunt meer dan één rol toewijzen aan een gebruiker en elke rol kan worden gekoppeld aan een ander bereik. Bijvoorbeeld:

* U toewijzen de lezer TestUser-A rol met abonnement 1.
* U toewijzen de eigenaar van TestUser-A rol aan TestVM-1.

De Azure [roltoewijzingen](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) artikel biedt gedetailleerde informatie over het weergeven, toewijzen en verwijderen van rollen.

### <a name="resource-hierarchy-and-inheritance"></a>Resource-hiërarchie en overname

Azure Stack heeft de volgende resource-hiërarchie:

* Elk abonnement behoort tot een map.
* Elke resourcegroep behoort tot één abonnement.
* Elke resource behoort tot één resourcegroep.

Toegang die u aan een bovenliggend bereik toewijst wordt overgenomen op onderliggende bereiken. Bijvoorbeeld:

* U toewijzen de **lezer** rol aan een Azure AD-groep op het abonnementsbereik. De leden van die groep kunnen elke resourcegroep en de resource weergeven in het abonnement.
* U toewijzen de **Inzender** rol bij een toepassing op het groepsbereik van de resource. De toepassing kunt resources van alle typen in die resourcegroep, maar geen andere resourcegroepen in het abonnement beheren.

### <a name="assigning-roles"></a>Toewijzen van rollen

U kunt meer dan één rol toewijzen aan een gebruiker en elke rol kan worden gekoppeld aan een ander bereik. Bijvoorbeeld:

* U toewijzen de lezer TestUser-A rol met abonnement 1.
* U toewijzen de eigenaar van TestUser-A rol aan TestVM-1.

De Azure [roltoewijzingen](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) artikel biedt gedetailleerde informatie over het weergeven, toewijzen en verwijderen van rollen.

## <a name="set-access-permissions-for-a-user"></a>Stel toegangsmachtigingen voor een gebruiker

De volgende stappen wordt beschreven hoe u machtigingen configureren voor een gebruiker.

1. Meld u aan met een account met eigenaarsmachtigingen voor de resource die u wilt beheren.
2. Kies **Resourcegroepen** in het linkernavigatievenster.
3. Kies de naam van de resourcegroep die u instellen van machtigingen wilt op.
4. Kies in het navigatiedeelvenster van de groep resource **toegangsbeheer (IAM)**. De **roltoewijzingen** weergave bevat de items die toegang tot de resourcegroep hebben. U kunt filteren en de resultaten groeperen.
5. Op de **toegangsbeheer** menu van de balk, kiest u **roltoewijzing toevoegen**.
6. Op **roltoewijzing toevoegen** deelvenster:

   * Selecteer de rol die u toewijzen wilt uit de **rol** vervolgkeuzelijst.
   * Kies de resource die u toewijzen wilt uit de **toegang toewijzen aan** vervolgkeuzelijst.
   * Selecteer de gebruiker, groep of toepassing in de directory aan wie/waaraan u toegang wilt verlenen. U kunt zoeken in de directory met weergavenamen, e-mailadressen en object-id's.

7. Selecteer **Opslaan**.

## <a name="next-steps"></a>Volgende stappen

[Service-principals maken](azure-stack-create-service-principals.md)