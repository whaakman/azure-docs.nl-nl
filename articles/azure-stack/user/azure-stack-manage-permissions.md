---
title: Machtigingen beheren voor bronnen per gebruiker in Azure-Stack | Microsoft Docs
description: Informatie over het beheren van RBAC machtigingen als een servicebeheerder of tenant.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: cccac19a-e1bf-4e36-8ac8-2228e8487646
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 026c686b1d5654aa50dd63b9addd619dd5322da0
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2018
ms.locfileid: "34808142"
---
# <a name="manage-access-to-resources-with-azure-stack-role-based-access-control"></a>Toegang tot bronnen met toegangsbeheer Azure Stack Role-Based beheren

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Azure-Stack biedt ondersteuning voor op rollen gebaseerde toegangsbeheer (RBAC), dezelfde [beveiligingsmodel voor access management](https://docs.microsoft.com/azure/role-based-access-control/overview) die gebruikmaakt van Microsoft Azure. U kunt gebruikmaken van RBAC voor het beheren van gebruikers, groepen of toegang tot de toepassing aan abonnementen, resources en services.

## <a name="basics-of-access-management"></a>Basisprincipes van beheer van toegang

Toegangsbeheer op basis van rollen biedt verfijnd toegangsbeheer die u gebruiken kunt voor het beveiligen van uw omgeving. Geeft u gebruikers de exacte machtigingen die ze nodig hebben door toe te wijzen een RBAC-rol op een bepaald bereik. Het bereik van de roltoewijzing is een abonnement, resourcegroep of één resource. Lees de [toegangsbeheer op basis van rollen in Azure portal](https://docs.microsoft.com/azure/role-based-access-control/overview) artikel voor meer gedetailleerde informatie over het toegangsbeheer van.

### <a name="built-in-roles"></a>Ingebouwde rollen

Azure Stack heeft drie elementaire functies die u op alle brontypen toepassen kunt:

* **Eigenaar** kunnen alles beheren, inclusief toegang tot bronnen.
* **Inzender** kunnen alles beheren behalve toegang tot bronnen.
* **Lezer** kunnen alles weergeven, maar u kunt geen wijzigingen aanbrengen.

### <a name="resource-hierarchy-and-inheritance"></a>Resource-hiërarchie en overname

Azure Stack heeft de volgende bron-hiërarchie:

* Elk abonnement behoort tot één map op.
* Elke resourcegroep behoort tot één abonnement.
* Elke resource behoort tot één resourcegroep.

Toegang waarmee u op een bovenliggend bereik wordt verleend wordt op een onderliggend bereik overgenomen. Bijvoorbeeld:

* U toewijzen de rol lezer aan een Azure AD-groep in het abonnementsbereik. De leden van die groep kunnen elk van de resourcegroep en bron weergeven in het abonnement.
* U toewijzen de rol van Inzender aan een toepassing op het groepsbereik resource. De toepassing kunt bronnen van alle typen in die resourcegroep, maar geen andere resourcegroepen in het abonnement te beheren.

### <a name="assigning-roles"></a>rollen toewijzen

U kunt meer dan één rol toewijzen aan een gebruiker en elke rol kan worden gekoppeld aan een ander bereik. Bijvoorbeeld:

* U de rol van de lezer TestUser-A toewijzen met abonnement 1.
* U toewijzen de eigenaar van TestUser-A rol aan TestVM-1.

De Azure [roltoewijzingen](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) artikel vindt u gedetailleerde informatie over het weergeven en verwijderen van rollen toewijzen.

### <a name="resource-hierarchy-and-inheritance"></a>Resource-hiërarchie en overname

Azure Stack heeft de volgende bron-hiërarchie:

* Elk abonnement behoort tot één map op.
* Elke resourcegroep behoort tot één abonnement.
* Elke resource behoort tot één resourcegroep.

Toegang waarmee u op een bovenliggend bereik wordt verleend wordt op een onderliggend bereik overgenomen. Bijvoorbeeld:

* U toewijzen de **lezer** rol bij het abonnementsbereik Azure AD-groep. De leden van die groep kunnen elk van de resourcegroep en bron weergeven in het abonnement.
* U toewijzen de **Inzender** role in een toepassing op het groepsbereik resource. De toepassing kunt bronnen van alle typen in die resourcegroep, maar geen andere resourcegroepen in het abonnement te beheren.

### <a name="assigning-roles"></a>rollen toewijzen

U kunt meer dan één rol toewijzen aan een gebruiker en elke rol kan worden gekoppeld aan een ander bereik. Bijvoorbeeld:

* U de rol van de lezer TestUser-A toewijzen met abonnement 1.
* U toewijzen de eigenaar van TestUser-A rol aan TestVM-1.

De Azure [roltoewijzingen](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) artikel vindt u gedetailleerde informatie over het weergeven en verwijderen van rollen toewijzen.

## <a name="set-access-permissions-for-a-user"></a>Stel toegangsmachtigingen voor een gebruiker

De volgende stappen beschrijven het configureren van machtigingen voor een gebruiker.

1. Aanmelden met een account met de van eigenaarsmachtigingen voor de resource die u wilt beheren.
2. Kies **Resourcegroepen** in het linkernavigatievenster.
3. Kies de naam van de resourcegroep die u machtigingen instellen wilt op.
4. Kies in het navigatiedeelvenster resource groep **toegangsbeheer (IAM)**. De **toegangsbeheer** weergave bevat de Items die toegang tot de resourcegroep hebben. U kunt deze resultaten filteren, en een menubalk toevoegen of verwijderen van machtigingen.
5. Op de **toegangsbeheer** menu balk, kiest u **+ toevoegen**.
6. Op **machtigingen toevoegen**:

   * Selecteer de rol die u toewijzen wilt uit de **rol** vervolgkeuzelijst.
   * Kies de resource die u toewijzen wilt uit de **toewijzen van toegang tot** vervolgkeuzelijst.
   * Selecteer de gebruiker, groep of toepassing in de directory aan wie/waaraan u toegang wilt verlenen. U kunt zoeken in de directory met weergavenamen, e-mailadressen en object-id's.

7. Selecteer **Opslaan**.

## <a name="next-steps"></a>Volgende stappen

[Service-principals maken](azure-stack-create-service-principals.md)