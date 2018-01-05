---
title: Beheren van toegang en machtigingen met RBAC - Azure RBAC | Microsoft Docs
description: In het beheer van toegang aan de slag met op rollen gebaseerde toegangsbeheer van Azure in de Azure Portal. Gebruik roltoewijzingen om machtigingen in uw directory toe te wijzen.
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
ms.assetid: 8f8aadeb-45c9-4d0e-af87-f1f79373e039
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/02/2018
ms.author: curtand
ms.reviewer: rqureshi
ms.openlocfilehash: ce9a9c95664a818919df756917180e102a5f1e0a
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="get-started-with-role-based-access-control-in-the-azure-portal"></a>Aan de slag met toegangsbeheer op basis van rollen in de Azure-portal
Beveiliging gerichte bedrijven moeten zich richten op uw werknemers de exacte machtigingen die ze nodig hebben. Te veel machtigingen kunnen een account dat kwaadwillende personen worden blootgesteld. Te weinig machtigingen betekent dat werknemers hun werk efficiënt kunnen niet ophalen. Azure op rollen gebaseerde toegangsbeheer (RBAC) kunt u dit probleem oplossen door het aanbieden van Geavanceerd toegangsbeheer voor Azure.

Met RBAC kunt u taken scheiden binnen uw team en de hoeveelheid toegang verlenen aan gebruikers die ze nodig hebben voor het uitvoeren van hun taken. In plaats van iedereen geven onbeperkte machtigingen in uw Azure-abonnement of de bronnen, kunt u alleen bepaalde acties. Bijvoorbeeld, gebruikmaken van RBAC te laten een werknemer virtuele machines in een abonnement beheren terwijl een andere SQL-databases binnen hetzelfde abonnement kunt beheren.

## <a name="basics-of-access-management-in-azure"></a>Basisprincipes van toegangsbeheer in Azure
Elk Azure-abonnement is gekoppeld aan één map op Azure Active Directory (AD). Gebruikers, groepen en toepassingen van die directory kunnen resources in het Azure-abonnement te beheren. Deze toegangsrechten met behulp van de Azure-portal, Azure-opdrachtregelprogramma's en Azure Management-API's toewijzen.

Toegang verlenen door de juiste RBAC-rol toewijzen aan gebruikers, groepen en toepassingen op een bepaalde scope. Het bereik van een roltoewijzing kan dit een abonnement, resourcegroep of één resource. Een rol die is toegewezen aan een bovenliggend bereik verleent toegang ook naar de onderliggende elementen erin opgenomen. Een gebruiker met toegang tot een resourcegroep kan bijvoorbeeld alle resources, zoals websites, virtuele machines en subnetten bevat te beheren.

![Relatie tussen de elementen van de Azure Active Directory - diagram](./media/role-based-access-control-what-is/rbac_aad.png)

De RBAC-rol die u toewijst, bepaalt welke bronnen de gebruiker, groep of toepassing binnen dat bereik kunt beheren.

## <a name="built-in-roles"></a>Ingebouwde rollen
Azure RBAC heeft drie elementaire functies die van toepassing op alle brontypen:

* **Eigenaar** heeft volledige toegang tot alle bronnen, waaronder het recht op toegang aan anderen delegeren.
* **Inzender** kunt maken en beheren van alle soorten Azure-resources, maar kan geen toegang tot de overige verlenen.
* **Lezer** bestaande Azure-resources kunt weergeven.

De rest van de RBAC-rollen in Azure toestaan van beheer van specifieke Azure-resources. De rol Inzender van de virtuele Machine kan bijvoorbeeld de gebruiker te maken en beheren van virtuele machines. Het biedt ze geen toegang geven tot het virtuele netwerk of het subnet dat de virtuele machine verbinding met maakt. 

[Ingebouwde RBAC-rollen](role-based-access-built-in-roles.md) vindt u de rollen die beschikbaar zijn in Azure. De Hiermee geeft u de bewerkingen en het bereik dat elke ingebouwde rol aan gebruikers verleent. Als u op zoek bent om te definiëren van uw eigen rollen voor nog meer informatie over het bouwen [aangepaste rollen in Azure RBAC](role-based-access-control-custom-roles.md).

## <a name="resource-hierarchy-and-access-inheritance"></a>Resource-hiërarchie en de toegang overname
* Elke **abonnement** behoort tot slechts één map in Azure. (Maar elke directory kan meer dan één abonnement hebt.)
* Elke **resourcegroep** behoort tot slechts één abonnement.
* Elke **resource** behoort tot slechts één resourcegroep.

Toegang waarmee u op de bovenliggende scopes wordt verleend wordt op een onderliggend bereik overgenomen. Bijvoorbeeld:

* U toewijzen de rol lezer aan een Azure AD-groep in het abonnementsbereik. De leden van die groep kunnen elk van de resourcegroep en bron weergeven in het abonnement.
* U toewijzen de rol van Inzender aan een toepassing op het groepsbereik resource. Deze kunt resources van alle typen in die resourcegroep, maar geen andere resourcegroepen in het abonnement beheren.

## <a name="azure-rbac-vs-classic-subscription-administrators"></a>Azure RBAC versus klassieke abonnementbeheerders
[Klassieke abonnementsbeheerders mensen en co-beheerders](../billing/billing-add-change-azure-subscription-administrator.md) hebben volledige toegang tot het Azure-abonnement. Ze kunnen resources met behulp van de [Azure-portal](https://portal.azure.com), Azure Resource Manager-API's en het klassieke implementatiemodel API's. In het model RBAC klassieke administrators de rol van eigenaar bij het abonnementsbereik toegewezen.

Alleen de Azure portal en de nieuwe Azure Resource Manager-API's ondersteunen Azure RBAC. Gebruikers en toepassingen die zijn toegewezen RBAC-rollen niet gebruiken de het Azure klassieke implementatiemodel API's.

## <a name="authorization-for-management-vs-data-operations"></a>Autorisatie voor het beheer van versus gegevensbewerkingen
Azure RBAC ondersteunt alleen beheerbewerkingen van de Azure-resources in de Azure-portal en Azure Resource Manager-API's. Deze kan niet toestaan dat alle gegevens niveau bewerkingen voor Azure-resources. Bijvoorbeeld, kunt u autoriseren iemand om de Storage-Accounts beheren, maar niet naar de BLOB's of tabellen binnen een Opslagaccount. Op deze manier kan een SQL-database worden beheerd, maar niet de tabellen in het.

## <a name="next-steps"></a>Volgende stappen
* Aan de slag met [toegangsbeheer op basis van rollen in Azure portal](role-based-access-control-configure.md).
* Zie de [Ingebouwde RBAC-rollen](role-based-access-built-in-roles.md)
* Definieer uw eigen [Aangepaste rollen in Azure RBAC](role-based-access-control-custom-roles.md)
